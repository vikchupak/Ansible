- https://habr.com/ru/companies/otus/articles/753642/

```yaml
- name: Start web app check in background
  command: /opt/monitor/checks.py
  async: 360
  poll: 0
  register: webapp_result

- name: Start DB check in background
  command: /opt/monitor/db.py
  async: 360
  poll: 0
  register: db_result

- name: Wait for web app check to finish
  async_status:
    jid: "{{ webapp_result.ansible_job_id }}"
  register: job_result
  until: job_result.finished
  retries: 36     # 36 * 10s = 360s, matches async timeout
  delay: 10
  failed_when: job_result.rc != 0
```

## 🔄 What Do `async` and `poll` Mean?

### ✅ `async: 360`

* async: Maximum time allowed for the background task to run on the remote node.
* This tells Ansible to allow the task to run **up to 360 seconds (6 minutes)** in the background.
* It’s the **timeout window** for how long the task is allowed to execute.

### ✅ `poll: 0`

* poll: 0: Fire and forget — Ansible won’t wait immediately.
* This tells Ansible **not to wait** for the task to finish.
* Ansible starts the task, then **immediately moves on** to the next one.
* The task runs **asynchronously**, in the background, on the remote node.

> This combo (`async: N`, `poll: 0`) is how you launch a task **in the background**.

### ✅ async_status module

async_status: Allows controlled polling and handling of completion or timeout.

---

## 📥 Example Breakdown

```yaml
- command: /opt/monitor/checks.py
  async: 360
  poll: 0
  register: webapp_result
```

* Launches `checks.py` in the background.
* Runs for up to 6 minutes.
* `webapp_result.ansible_job_id` will store the **job ID** of the background task.

---

## 🔍 Checking Task Status

```yaml
- name: Wait for web app check to finish
  async_status:
    jid: "{{ webapp_result.ansible_job_id }}"
  register: job_result
  until: job_result.finished
  retries: 36     # 36 * 10s = 360s, matches async timeout
  delay: 10
  failed_when: job_result.rc != 0
```

* This uses the `async_status` module to **check whether the background job is done**.
* It checks the status of the job with the given `job_id`.

### 1. **`until: job_result.finished`**

* Keeps checking until the job completes.

### 2. **`retries: 36`, `delay: 10`**

* Total polling time = `36 × 10 = 360s`, which **matches** the `async` timeout.
* Prevents polling from ending too early or too late.

### 3. **`failed_when: job_result.rc != 0`**

* Explicitly fails the task if the background job **completes with a non-zero exit code**.
* Without this, Ansible may report the polling task as “successful” even if the background job failed.

> 💡 `job_result.rc` contains the exit code of the background job. Standard UNIX rule: `0` means success, anything else means failure.

---

## 🧠 Summary

| Option         | Purpose                                          |
| -------------- | ------------------------------------------------ |
| `async: 360`   | Run the task for up to 360 seconds.              |
| `poll: 0`      | Don’t wait; run task in the background.          |
| `async_status` | Later check if the background task has finished. |

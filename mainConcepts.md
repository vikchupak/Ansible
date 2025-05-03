<details>
<summary>
  <strong>
    modules, tasks, plays, playbooks
  </strong> 
</summary>
<br />

In Ansible, **modules**, **tasks**, **plays**, and **playbooks** are key concepts used to define and execute automation workflows. Here's a detailed explanation of each:

-----

### **1. Modules**
- **Definition**: Modules are standalone units of code that perform specific tasks. They are the building blocks of Ansible automation.
- **Purpose**: Modules abstract complex operations into reusable, declarative components. You use them to define "what" you want to do, while the module handles "how" it’s done.
- **Examples**:
  - **System management**: `apt`, `yum`, `package`
  - **File operations**: `copy`, `file`, `template`
  - **Cloud services**: `ec2`, `azure_rm`
  - **Commands**: `shell`, `command`
- **How to Use**: Modules are executed within tasks (explained below).
- **Example**:
  ```yaml
  - name: Install Apache
    apt:
      name: apache2
      state: present
  ```

**`Gather Facts` module:**
- Is automatically added and executed at the beginning of every play
- Collects info/vars about remote servers that can be used in playbooks

---

### **2. Tasks**
- **Definition**: A task is a single action or step that uses a module to perform a specific function on a target host.
- **Purpose**: Tasks define what needs to be done and in what order, such as installing packages, copying files, or restarting services.
- **Characteristics**:
  - Tasks are executed sequentially, one after another.
  - Tasks can include conditional logic, loops, and error handling.
- **Example**:
  ```yaml
  - name: Update and install Apache
    apt:
      name: apache2
      state: present
      update_cache: yes

  - name: Copy custom configuration file
    copy:
      src: /local/path/apache.conf
      dest: /etc/apache2/apache.conf
  ```

---

### **3. Plays**
- **Definition**: A play is a collection of tasks that target a specific group of hosts.
- **Purpose**: A play maps tasks to groups of hosts in your inventory, defining which tasks should run on which hosts.
- **Characteristics**:
  - Plays specify:
    - The `hosts` the tasks apply to.
    - Privilege escalation (`become` for `sudo`/root access).
    - Variables, roles, and more.
  - Plays can include multiple tasks executed in sequence.
- **Example**:
  ```yaml
  - name: Configure web servers
    hosts: web_servers
    become: yes

    tasks:
      - name: Install Apache
        apt:
          name: apache2
          state: present
  ```

---

### **4. Playbooks**
- **Definition**: A playbook is a YAML file containing one or more plays. It is the top-level configuration file that defines your entire automation workflow.
- **Purpose**: Playbooks orchestrate the execution of tasks across multiple hosts and groups, specifying the desired state of the system.
- **Characteristics**:
  - Playbooks allow you to:
    - Define roles, variables, and handlers.
    - Manage multiple systems at once.
    - Include advanced logic, such as conditionals, loops, and error handling.
  - Playbooks are reusable and modular.
- **Example**:
  ```yaml
  ---
  - name: Configure web servers
    hosts: web_servers
    become: yes

    tasks:
      - name: Install Apache
        apt:
          name: apache2
          state: present

      - name: Start Apache service
        service:
          name: apache2
          state: started
  ```

---

### **How They Work Together**
- **Modules**: Do the actual work (e.g., install Apache, copy files).
- **Tasks**: Use modules to define individual steps.
- **Plays**: Group tasks and specify which hosts they run on.
- **Playbooks**: Orchestrate plays to achieve end-to-end automation.

---

### **Example Workflow**
Here’s how all these concepts work together:

#### Inventory File (`inventory/hosts`)
```ini
[web_servers]
web1.example.com
web2.example.com
```

#### Playbook (`site.yml`)
```yaml
- name: Configure web servers
  hosts: web_servers
  become: yes

  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install Apache
      apt:
        name: apache2
        state: present

    - name: Start Apache service
      service:
        name: apache2
        state: started
```

#### Execution
Run the playbook:
```bash
ansible-playbook -i inventory/hosts site.yml
```

**Result**:
- Ansible connects to all `web_servers` in the inventory.
- It executes each task (update cache, install Apache, start service) sequentially.
- Each task uses a specific module to perform the action.

---

### **Summary Table**
| Concept      | What It Does                                  | Example                                         |
|--------------|-----------------------------------------------|------------------------------------------------|
| **Module**   | Performs a specific function (e.g., install a package, copy a file). | `apt`, `service`, `copy`                      |
| **Task**     | Uses a module to define a single action.      | `Install Apache using apt`                    |
| **Play**     | Groups tasks and maps them to hosts.          | `Configure web_servers`                       |
| **Playbook** | Orchestrates multiple plays to define the entire workflow. | `site.yml` with tasks for web and database servers |

</details>

<details>
<summary>
  <strong>
    Sequence/Order of execution
  </strong> 
</summary>
<br />

- Within a Playbook, plays are executed sequentially in the order they are defined in the playbook.
- Within a Play, tasks are executed sequentially, in the order they are defined in the play.
- Each task is applied to all hosts in the play **in parallel across hosts**, and Ansible waits for all hosts to complete the task before moving to the next task.

</details>

<details>
<summary>
  <strong>
    Files
  </strong> 
</summary>
<br />

- In Ansible, there is no separate file for individual plays. Plays are always part of a playbook, which groups them together.
- A playbook must have at least one play, and plays define which tasks to run.
- You can organize your tasks into separate task files and reuse them across multiple playbooks or plays using `include_tasks` or `import_tasks`.
  - But, tasks in Ansible cannot be run on their own outside of a playbook or a role.
- If you need to separate the plays into different files for organizational purposes, you can use `import_playbook`, which allows you to combine multiple playbooks, but this does not allow you to include individual plays.

  - `web_servers.yml`:
      ```yaml
      ---
      - name: Configure web servers
        hosts: web_servers
        tasks:
          - name: Install Apache
            apt:
              name: apache2
              state: present
      ```
  
  - `db_servers.yml`:
      ```yaml
      ---
      - name: Configure database servers
        hosts: db_servers
        tasks:
          - name: Install PostgreSQL
            apt:
              name: postgresql
              state: present
      ```
  
  - Main Playbook: `site.yml`
      ```yaml
      ---
      - import_playbook: web_servers.yml
      - import_playbook: db_servers.yml
      ```

</details>

<details>
<summary>
  <strong>
    Ad Hoc Tasks
  </strong> 
</summary>
<br />

Ad hoc is a Latin phrase that means "for this purpose" or "for a specific task."

In Ansible Context, an ad hoc task is a task that is executed immediately and directly, often without the need to write a full playbook.

Run Ad Hoc task using ping module
```bash
ansible all -m ping
```

</details>

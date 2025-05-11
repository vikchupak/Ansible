# Vars name convention

- `linux_user` or `linuxuser` - fine (letters, numbers, underscores)
- `linux-user`, `linux.user`, `5linux_user` (number first), `linux user` - Won't work

# Registered variables (`module execution results`)

- Create variables from output of Ansible tasks (**output of module execution - return values**) using `regiter` attribute. These vars can be used in later tasks in your play. To print registered vars, use `debug` module.

# Referencing variables

Can be defined in:
- Playbook's play
- Pass vars as command line args (vars accessible for the whole playbook in all its plays)
  ```bash
  ansible-playbook -i hosts <playbook-name>.yaml --extra-vars "var1=value1 var2=value2"
  ```
- Separate yaml file (any name) and use `vars_file` attribute in all the playbook's plays

---

In **Ansible**, variables can be defined in many places, and Ansible uses a **defined precedence order** (priority) to decide which value to use when the same variable is defined in multiple locations.

Here is the **official variable precedence hierarchy** from **lowest to highest**:

## Ansible Variable Precedence (Lowest to Highest)

| Precedence | Source                                  | Notes                                                        |
| ---------- | --------------------------------------- | ------------------------------------------------------------ |
| 1️⃣        | **Role defaults** (`defaults/main.yml`) | Lowest priority — intended as fallback defaults inside roles |
| 2️⃣        | **Inventory file variables**            | Defined in `inventory.ini` or YAML-based inventory           |
| 3️⃣        | **Inventory group vars**                | `group_vars/all.yml`, `group_vars/webservers.yml`, etc.      |
| 4️⃣        | **Inventory host vars**                 | `host_vars/hostname.yml`                                     |
| 5️⃣        | **Playbook `vars:` block**              | Vars set directly inside the playbook                        |
| 6️⃣        | **Playbook `vars_files:`**              | External files loaded using `vars_files`                     |
| 7️⃣        | **Playbook `vars_prompt:`**             | User input at runtime                                        |
| 8️⃣        | **Set in a task using `set_fact`**      | Runtime variable, higher precedence                          |
| 9️⃣        | **Registered variables**                | Values returned from tasks                                   |
| 🔟         | **Extra vars** (`-e`)                   | Highest priority — overrides everything else                 |

![image](https://github.com/user-attachments/assets/fc32ceef-f56d-4ae5-b83b-7392cd6bded6)

# Tricky case

In yaml files, we need to use double quetes `""` to wrap variables that directly follow collums `:` in order to say that it is ansible ref var, not yaml dictionary.

```yaml
# Good
src: "{{location}}/nodejs-app-{{version}}.tgz"
src: /root/nodejs-app-{{version}}.tgz

# Bad
src: {{location}}/nodejs-app-{{version}}.tgz
```

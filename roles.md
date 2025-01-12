# Roles

- Like a package/bundle of your **tasks**
- Only for tasks and **its files**(like static files, variables for tasks, custom modules). So the roles are packaged to contain everything they need.
- Extract tasks from different plays to reuse them in different playbooks/plays.
- **Roles have standard defined structure**
- Use existing Roles from community
  - From `ansible-galaxy`
  - From `git repositories`

### Usage

Just replace `tasks` attribute with `roles` attribute in a playbook's plays.

### **Directory Structure of a Role**

A typical Ansible role has the following directory structure:

```
roles/
  my_role/
    defaults/    # Default variables (lowest priority)
    vars/        # Role-specific variables (higher priority than defaults)
    tasks/       # Main tasks to execute (mandatory)
    handlers/    # Handlers for service notifications
    files/       # Static files to copy to remote hosts
    templates/   # Jinja2 templates to dynamically generate files
    meta/        # Metadata about the role (e.g., dependencies)
    tests/       # Test playbooks for the role
```

#### **Key Directories and Their Purpose**
- **`defaults/`**:
  - Contains default variables for the role (lowest priority in the variable precedence hierarchy).
  - Example: `roles/my_role/defaults/main.yml`

- **`vars/`**:
  - Contains variables specific to the role (higher priority than `defaults`).
  - Example: `roles/my_role/vars/main.yml`

- **`tasks/`**:
  - Contains the main list of tasks to execute for the role.
  - Example: `roles/my_role/tasks/main.yml`
  - Example content:
    ```yaml
    ---
    - name: Install nginx
      apt:
        name: nginx
        state: present
    ```

- **`handlers/`**:
  - Contains handlers that are triggered by tasks (e.g., restarting a service).
  - Example: `roles/my_role/handlers/main.yml`
  - Example content:
    ```yaml
    ---
    - name: Restart nginx
      service:
        name: nginx
        state: restarted
    ```

- **`files/`**:
  - Contains static files that can be copied to the remote hosts.
  - Example: `roles/my_role/files/my_config.conf`

- **`templates/`**:
  - Contains Jinja2 templates for generating dynamic configuration files.
  - Example: `roles/my_role/templates/my_config.j2`

- **`meta/`**:
  - Contains metadata about the role, such as dependencies on other roles.
  - Example: `roles/my_role/meta/main.yml`
  - Example content:
    ```yaml
    ---
    dependencies:
      - role: other_role
    ```

- **`tests/`**:
  - Contains test playbooks and resources to test the role.
  - Example: `roles/my_role/tests/test.yml`

### **How to Create a Role**

You can manually create the directory structure or use the **`ansible-galaxy init`** command to generate it automatically.

#### **Command to Create a Role:**
```bash
ansible-galaxy init my_role
```

This creates the role directory structure under the `my_role` folder.

---

### **Using Roles in Playbooks**

To use a role in a playbook, you reference it under the `roles` keyword.

#### **Example Playbook Using Roles:**
```yaml
- name: Apply roles to all hosts
  hosts: all
  roles:
    - role: web_server
    - role: database_server
```

In this example:
- `web_server` and `database_server` are directories under the `roles/` directory.

**You can mix roles and tasks in the same playbook.**

- `roles` are executed before `tasks` listed in the tasks section of the same play
- `pre_tasks` are executed before `roles`
- `post_tasks` are executed after `roles`

---

### **Role Variables and Overrides**

- Variables in a role can be defined in:
  - `defaults/main.yml`: Lowest priority.
  - `vars/main.yml`: Higher priority.
  - Playbook or inventory: Overrides role variables.

#### **Example of Default Variables:**
```yaml
# roles/my_role/defaults/main.yml
nginx_port: 80
nginx_user: www-data
```

You can override these in a playbook:
```yaml
- name: Override variables
  hosts: all
  roles:
    - role: my_role
      vars:
        nginx_port: 8080
```

---

### **Role Dependencies**

You can define dependencies in the `meta/main.yml` file. Dependencies are other roles that must be applied before the current role.

#### **Example of Role Dependencies:**
```yaml
# roles/my_role/meta/main.yml
---
dependencies:
  - role: base
  - role: firewall
    vars:
      firewall_allowed_ports:
        - 22
        - 80
```

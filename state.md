# State

- Ansible is a **stateless automation tool**, meaning it doesn’t maintain or store a persistent state of the systems it manages.
- Instead, **it determines the current state of a system dynamically** by executing tasks during each run and assessing their outcomes.

### **Idempotent Modules**
   - Most Ansible modules are idempotent, meaning they ensure the system reaches the desired state without causing changes if the system is already in that state.
   - For example:
     ```yaml
     - name: Ensure Apache is installed
       yum:
         name: httpd
         state: present
     ```
     If Apache is already installed, this task will do nothing; otherwise, it installs it.

### **Facts Gathering**
   - Ansible collects **facts** about the target system (e.g., OS, IP addresses, installed software) at the beginning of a playbook run using the `setup` module.
   - These facts allow Ansible to make decisions dynamically during execution.
   - Example:
     ```yaml
     - debug:
         var: ansible_facts['os_family']
     ```

### **Check Mode**
   - Ansible can run in **check mode** (using the `--check` flag), where it simulates changes without making any actual modifications. This helps predict the changes without affecting the system.

While Ansible itself does not maintain a centralized state, it relies on dynamic checks and idempotent behavior to ensure systems converge to the desired state during each playbook execution. If persistent state tracking is necessary, you can integrate tools like Ansible Tower, AWX, or external databases.

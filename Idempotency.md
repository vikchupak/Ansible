# Idempotency in Ansible

Idempotency means that running the same operation multiple times produces the same result and has no unintended side effects. In Ansible, it ensures that a task only makes changes when necessary, leaving the system in the desired state regardless of how many times the playbook is executed.

### **How Idempotency Works in Ansible**

- Ansible modules are designed to be idempotent by default.  
- Before performing an action, modules check the current state of the system and only make changes if the desired state is not already met.

For example:
- If a file already exists with the correct permissions and content, the `file` or `copy` module does nothing.
- If a package is already installed, the `apt` or `yum` module skips the installation.

---

### **When Idempotency Might Be Broken**

1. **Using Non-Idempotent Modules**:  
   Some modules, like `shell` and `command`, are not inherently idempotent because they execute commands blindly.

   ```yaml
   - name: Restart Apache (non-idempotent)
     command: systemctl restart apache2
   ```
   - This will restart Apache every time the playbook is run, even if unnecessary.

2. **Lack of State Checking**:  
   If a task does not check for the current state of the system before performing an action, it may repeatedly apply changes.

   Example:
   ```yaml
   - name: Add a line to a file (non-idempotent)
     shell: echo "line" >> /etc/example.conf
   ```
   - This appends "line" every time the playbook is executed.

   **Solution**: Use the `lineinfile` module instead, which ensures the line exists without duplication:
   ```yaml
   - name: Ensure a line is present in the file
     lineinfile:
       path: /etc/example.conf
       line: "line"
   ```

3. **Custom Scripts**:  
   If you use custom scripts that don't account for the current state, the task won't be idempotent unless you add logic to check the state.

---

### **How to Ensure Idempotency**

1. **Use Idempotent Modules**:  
   Rely on Ansible modules like `apt`, `yum`, `copy`, `file`, `service`, etc., which are designed for idempotency.

2. **Avoid `command` and `shell` Modules**:  
   If you must use them, ensure you add checks to make them idempotent.

   https://gitlab.com/twn-devops-bootcamp/latest/15-ansible/ansible-projects/-/blob/main/deploy-nexus.yaml?ref_type=heads#L29

   Example:
   ```yaml
   - name: Create a directory if it doesn't exist (idempotent shell)
     shell: |
       if [ ! -d "/mydir" ]; then
         mkdir /mydir
       fi
   ```

4. **Use Conditional Logic (`when`)**:  
   Prevent tasks from running unnecessarily.
   ```yaml
   - name: Restart Apache if configuration changed
     service:
       name: apache2
       state: restarted
     when: apache_config_changed
   ```

5. **Use Handlers**:  
   Trigger actions (like restarting services) only when necessary changes occur.

6. **Test Playbooks**:  
   Run playbooks multiple times to confirm idempotency and debug tasks that apply changes unnecessarily.

---

### **Conclusion**

Idempotency is a critical feature in Ansible, allowing you to run playbooks multiple times without causing unintended changes. By using idempotent modules and writing tasks carefully, you ensure consistency, safety, and efficiency in your automation workflows.

`env` plugin to read env vars
- https://docs.ansible.com/ansible/latest/collections/ansible/builtin/env_lookup.html

The `ansible_env` variable provides access to the environment variables of the **remote host**.

Writing a Custom Lookup Plugin for `env`

File: lookup_plugins/custom_env.py
```python
from ansible.plugins.lookup import LookupBase
import os

class LookupModule(LookupBase):
    def run(self, terms, variables=None, **kwargs):
        result = []
        for term in terms:
            # Fetch the environment variable or return a default value
            value = os.getenv(term, kwargs.get('default', None))
            result.append(value)
        return result
```
Usage
```yaml
- name: Use a custom env lookup
  hosts: localhost
  tasks:
    - debug:
        msg: "Custom HOME is {{ lookup('custom_env', 'HOME', default='/default/home') }}"
```

---

One more example of plugin usage (dynamic inventory)
- https://github.com/VIK2395/Ansible/blob/main/inventory/dynamicInventory.md

---

**Ansible plugins** are typically written in **Python**. Ansible is built in Python, and the majority of its core components (including plugins) are also written in Python. This allows for easy integration and extensibility within Ansible.

There are several types of plugins, and they are all written in Python. Here's a breakdown of the most common types of plugins in Ansible and their typical usage:

---

### **Types of Ansible Plugins**

1. **Lookup Plugins**
   - Used to fetch data from an external source, such as environment variables, files, or APIs.
   - Example: `env`, `file`, `pipe`.

2. **Filter Plugins**
   - Used to modify or transform data (such as variables or outputs).
   - Example: `default`, `json_query`, `regex_replace`.

3. **Action Plugins**
   - Define the behavior of tasks (for example, how to run a command or copy a file).
   - Example: `command`, `shell`, `copy`.

4. **Connection Plugins**
   - Define how Ansible connects to remote hosts (e.g., over SSH, Docker, WinRM).
   - Example: `ssh`, `local`, `docker`.

5. **Inventory Plugins**
   - Used to dynamically fetch inventory from sources other than static files (e.g., databases, cloud providers, etc.).
   - Example: `ec2`, `yaml`, `ini`.

6. **Callback Plugins**
   - Used to handle logging or output formatting, such as printing results in JSON or human-readable formats.
   - Example: `json`, `yaml`, `profile_tasks`.

7. **Strategy Plugins**
   - Define the execution strategy for playbooks (e.g., run tasks in parallel, or execute in a linear sequence).
   - Example: `linear`, `free`.

8. **Vars Plugins**
   - Provide additional variable processing logic for Ansible, like fetching values dynamically.
   - Example: `env`, `fact`, `host`.

---

### **Creating a Custom Ansible Plugin**

1. **Plugin Directory**: Your custom plugin should reside in a directory like `lookup_plugins/`, `filter_plugins/`, or others depending on the plugin type.
   
2. **Python Class**: The plugin should define a class (e.g., `LookupModule`, `ActionModule`) that extends the relevant base class for that plugin type.

3. **Required Methods**: The class must implement certain methods. For example, for a **lookup plugin**, you must define a `run()` method, and for an **action plugin**, you must define an `execute()` method.

4. **Metadata**: Optionally, provide metadata about the plugin, including the description, author, and other relevant information.

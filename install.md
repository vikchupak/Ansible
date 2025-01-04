Official installation doc
- https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html

You need Python to run and install Ansible. Ansible is written in Python and requires it to function.

## Using a Package Manager (e.g., `apt`):
   - On some Linux distributions, you can install Ansible directly using a package manager. These packages often depend on Python and will automatically install it if needed.
   - If you install Ansible using ``apt, it will be **installed system-wide, not in a virtual environment**.
   - **If you want to run Ansible in a virtual environment, you must explicitly create one and install Ansible via `pip` instead.**

   Example (Ubuntu/Debian):
   ```bash
   sudo apt update
   sudo apt install ansible
   ```

   Will use already installed system-wide python version https://github.com/VIK2395/Python/blob/main/version_management.md

## Using python

After installing python

```bash
pip3 install ansible
```

**Virtual Environments (Optional)**

It's a good practice to use a Python virtual environment to install Ansible, ensuring it doesn't interfere with system-wide packages:
```bash
python3 -m venv ansible-env
source ansible-env/bin/activate
pip install ansible
```

**Check if Ansible is in a Virtual Environment**
```bash
ansible --version
```
```bash
ansible [core 2.14.5]
config file = /etc/ansible/ansible.cfg
configured module search path = ['/home/user/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
ansible python module location = /usr/lib/python3/dist-packages/ansible
executable location = /usr/bin/ansible
python version = 3.10.6 (default, Dec  7 2022, 13:24:06) [GCC 11.3.0]
```

 - If the Python executable path is something like `/usr/bin/python3`, Ansible is running system-wide.
 - If the path points to something like `/home/user/.venv/bin/python3`, Ansible is running inside a virtual environment.

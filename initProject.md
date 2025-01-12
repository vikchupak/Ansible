# Init project with python and virtual env

https://github.com/VIK2395/ansible_project

After python installation

```bash
mkdir ansible && cd ansible
```

```bash
python3 -m venv venv
```

```bash
source venv/bin/activate
```

```bash
pip install ansible
```

```bash
pip freeze > requirements.txt
```

# Example of 'good' project structure

```
ansible-project/
├── ansible.cfg
├── inventory/
│   ├── hosts
│   ├── group_vars/
│       ├── web_servers.yml
│   ├── host_vars/
│       ├── host1.yml
├── playbooks/
│   ├── site.yml
├── roles/
│   ├── apache/
│       ├── tasks/
│       │   ├── main.yml
│       ├── templates/
│       │   ├── index.html.j2
│       ├── vars/
│       │   ├── main.yml
│       ├── defaults/
│       │   ├── main.yml
│       ├── handlers/
│       │   ├── main.yml
```

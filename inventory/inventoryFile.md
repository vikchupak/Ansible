```ini
[webservers] # host group
192.168.1.10
192.168.1.11

[webservers:vars]
ansible_ssh_private_key=~/.ssh/id_rsa
ansible_user=root
ansible_python_interpreter=/usr/bin/python3.9 # To explicitly set path to the interpreter as it may change 

192.168.1.20 # single host
```

The default location of the Ansible inventory file, **commonly referred to as the "hosts file"**, is:
```bash
/etc/ansible/hosts
```

Example running the ansible on target servers
```bash
ansible webservers -i ./hosts -m ping
```
- `webservers` - host group
- `-i ./hosts` - specifies the custom inventory file to use
- `-m ping` - uses the ping ansible module to test connectivity

On target servers must be installed python. And we can specify path to it.
![image](https://github.com/user-attachments/assets/9260d59d-3b49-4cc6-abe0-79cdbe8d2817)

```ini
[webservers]
192.168.1.10
192.168.1.11

[webservers:vars]
ansible_ssh_private_key=~/.ssh/id_rsa
ansible_user=root

[databases]
192.168.1.20
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

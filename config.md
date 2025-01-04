# Ansible config files location

In new ansible versions, these folders/files are not created when installing. So we have to create them yourselves.

System-wide default
```
/etc/ansible/ansible.cfg
```

User-specific
```
~/.ansible/ansible.cfg
```

Common settings example
```ini
[defaults]
inventory = ./inventory       # Path to inventory file
remote_user = ansible         # Default remote user
host_key_checking = False     # Disable host key checking
forks = 10                    # Number of parallel processes
log_path = ./ansible.log      # Path to log file

[privilege_escalation]
become = True                 # Enable privilege escalation
become_method = sudo          # Escalation method
become_user = root            # User to escalate to
```

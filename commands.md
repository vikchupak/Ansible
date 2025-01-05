Run playbook
```bash
ansible-playbook -i inventory/hosts [--check] <playbook-file>.yml
```
- `--check` - Check mode (dry run). To see what changes would be made without applying them

Run Ad Hoc task using ping module
```bash
ansible all -m ping
```

Install collection
```bash
ansible-galaxy collection install <collection>
```

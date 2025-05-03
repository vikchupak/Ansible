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

Validate/preview inventory (file)
```bash
ansible-inventory -i inventory_aws_ec2.yaml [--list|--graph]
```

---

`command` vs `shell`

`shell` command can do everithing that `command` can and more. But it is better to use `command` for tasks that do not require extra functionality that `shell` provides because of security reasons.

Run playbook
```bash
ansible-playbook -i inventory/hosts <playbook-file>.yml
```

- In Ansible, there is no separate file for individual plays. Plays are always part of a playbook, which groups them together.
- A playbook must have at least one play, and plays define which tasks to run.
- You can organize your tasks into separate task files and reuse them across multiple playbooks or plays using `include_tasks`.
  - But, tasks in Ansible cannot be run on their own outside of a playbook or a role.

We should connect to target server as root user. Otherwise, we will face the following issues [limitations of become
](https://docs.ansible.com/ansible-core/2.17/playbook_guide/playbooks_privilege_escalation.html#risks-and-limitations-of-become)

- Example, AWS EC2 instances are connected via ssh as non-root users (like, ec2-user, ubuntu), which are not privileged.
  - Plays are run as users, not root user. We can add `become: true` to switch to root.
  - But still, if we create another non-root user (viktor) that user won't have enough permissions to make changes to the system.

---

- Use `become: true` WITHOUT `become_user` to escalate to root privileges. If set at the play level, it applies to all tasks in that play
- **The playbook runs as ssh user by default**
- By using `become_user`, you ensure that the playbook runs as the specified user_name

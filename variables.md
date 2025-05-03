# Vars name convention

- `linux_user` or `linuxuser` - fine (letters, numbers, underscores)
- `linux-user`, `linux.user`, `5linux_user` (number first), `linux user` - Won't work

# Registered variables (`module execution results`)

- Create variables from output of Ansible tasks (**output of module execution - return values**) using `regiter` attribute. These vars can be used in later tasks in your play. To print registered vars, use `debug` module.

# Referencing variables

Can be defined in:
- Playbook's play
- Pass vars as command line args (vars accessible for the whole playbook in all its plays)
  ```bash
  ansible-playbook -i hosts <playbook-name>.yaml --extra-vars "var1=value1 var2=value2"
  ```
- Separate yaml file (any name) and use `vars_file` attribute in all the playbook's plays

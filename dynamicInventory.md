# Dynamic inventory

We need a way to dynamicly get target servers for inventory file.

Off doc
- https://docs.ansible.com/ansible/latest/inventory_guide/intro_dynamic_inventory.html

Possible ways:
- `Inventory plugins`
  - Written in yaml
  - Make use of all ansible features, like
    - state management
- `Inventory scripts`
  - Written in python

Inventory plugins are preferred over Inventory scripts per ansible doc.

Plugins and scripts are specific for infrastructure provider.

https://docs.ansible.com/ansible/latest/collections/amazon/aws/aws_ec2_inventory.html

Enable plugin in `ansible.cfg` file and configure vars
```ini
enable_plugins = aws_ec2

remote_user = ec2-user
private_key_file = ~/.ssh/id_rsa
```
and create plugin config file `inventory_aws_ec2.yaml`. It must end with `aws_ec2.yaml`
```yaml
plugin: aws_ec2
regions: 
  - eu-central-1
```

Use plugin to get all EC2's metadata under `aws_ec2` hosts group
```bash
ansible-inventory -i inventory_aws_ec2.yaml [--list|--graph]
```

Use inventory plugin to run ansible playbook against ec2 dynamic `aws_ec2` hosts group
```bash
ansible-playbook -i inventory_aws_ec2.yaml ansible_plabook.yaml
```

We can set plugin inventory file as default in `ansible.cfg` file to not set `-i inventory_aws_ec2.yaml` in `ansible-playbook` command8 every time
```ini
inventory = inventory_aws_ec2.yaml
```

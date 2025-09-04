# 05 - Adhoc Commands in Ansible

Adhoc commands are one-liner commands used to quickly perform tasks without writing full playbooks.

## Examples

### Check connectivity (ping)
```bash
ansible all -m ping
```

### Copy a file
```bash
ansible all -m copy -a "src=/etc/hosts dest=/tmp/hosts" -b
```

### Create a file
```bash
ansible all -m file -a "path=/tmp/demo.txt state=touch" -b
```

### Install a package
```bash
ansible all -m yum -a "name=git state=present" -b
```

### Create a user
```bash
ansible all -m user -a "name=saipraneeth state=present" -b
```
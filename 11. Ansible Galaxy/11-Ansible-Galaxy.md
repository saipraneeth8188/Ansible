# 11. Ansible Galaxy

## What is Ansible Galaxy?
Ansible Galaxy is a hub for finding, sharing, and reusing Ansible roles. It provides both a command-line tool and a web interface to discover and download high-quality roles.

## Using Ansible Galaxy CLI

### To Install a Role
```bash
ansible-galaxy install geerlingguy.apache
```

### To View Installed Roles
```bash
ansible-galaxy list
```

### To Remove a Role
```bash
ansible-galaxy remove geerlingguy.apache
```

## Creating and Sharing a Role
```bash
ansible-galaxy init myrole
# Then push to a GitHub repo and link to Galaxy.
```

## Directory
Installed roles are usually saved in `~/.ansible/roles/` unless specified otherwise.

---
Ansible Galaxy accelerates playbook development by leveraging community-tested roles.
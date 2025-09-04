# 09 - Tags and Debug Module

## Tags
Used to run only specific tasks in a playbook.

```yaml
tasks:
  - name: Install NGINX
    yum:
      name: nginx
      state: present
    tags: install
```

Run with:
```bash
ansible-playbook playbook.yml --tags install
```

## Debug Module

```yaml
- debug:
    msg: "Welcome to Ansible"
```
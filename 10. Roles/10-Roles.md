# 10. Roles in Ansible

## Why Roles?
Roles help in organizing playbooks and making reusable components. Instead of putting all tasks, variables, handlers, templates, and files in one large playbook, roles divide them into standardized directory structures.

## Role Directory Structure
```
roles/
├── webserver/
│   ├── tasks/
│   │   └── main.yml
│   ├── handlers/
│   │   └── main.yml
│   ├── templates/
│   │   └── httpd.conf.j2
│   ├── files/
│   │   └── index.html
│   ├── vars/
│   │   └── main.yml
│   ├── defaults/
│   │   └── main.yml
│   ├── meta/
│   │   └── main.yml
```

## Key Directories Explained
- `tasks/`: List of tasks to execute.
- `handlers/`: Handler tasks triggered via notify.
- `templates/`: Jinja2 template files.
- `files/`: Static files to copy.
- `vars/`: Variables with high precedence.
- `defaults/`: Lowest priority default variables.
- `meta/`: Role dependencies.

## Using a Role in a Playbook
```yaml
- hosts: webservers
  roles:
    - webserver
```

## Creating a Role
```bash
ansible-galaxy init <role-name>
```

## Example
```bash
ansible-galaxy init apache
# Edits can be done in tasks/main.yml, etc.
```

---
Roles make your playbooks clean, reusable, and easier to maintain.
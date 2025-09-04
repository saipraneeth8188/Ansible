# 09 - Roles and Ansible Galaxy

## 🔹 What is a Role?

A **Role** is a structured way to organize playbooks, tasks, variables, handlers, and templates. It promotes reuse and modular design.

---

## 🔹 Directory Structure of a Role

```bash
myrole/
├── defaults/
│   └── main.yml
├── files/
├── handlers/
│   └── main.yml
├── meta/
│   └── main.yml
├── tasks/
│   └── main.yml
├── templates/
├── vars/
│   └── main.yml
```

---

## 🔹 Creating a Role

```bash
ansible-galaxy init myrole
```

---

## 🔹 Using a Role in a Playbook

```yaml
- hosts: web
  roles:
    - myrole
```

---

## 🔹 Roles with Variables

Pass variables using `vars`:

```yaml
- hosts: db
  roles:
    - role: mysql
      vars:
        mysql_root_password: admin123
```

---

## 🔹 Role Search Order

Ansible looks for roles in:

1. `roles_path` (defined in ansible.cfg)
2. `roles/` in current directory
3. `/etc/ansible/roles/`

---

## 🔹 Using Ansible Galaxy

Ansible Galaxy is a public repository for sharing roles.

### Install a Role:

```bash
ansible-galaxy install geerlingguy.apache
```

### Requirements File:

```yaml
# requirements.yml
- src: geerlingguy.mysql
  version: "3.3.0"
```

```bash
ansible-galaxy install -r requirements.yml
```

---

## 🔹 Meta Information in Roles

Defined in `meta/main.yml`:

```yaml
dependencies:
  - role: common
  - role: nginx
```

---

## 🔹 Best Practices

✅ Use roles to modularize playbooks  
✅ Store reusable roles in a central directory  
✅ Document your roles in `README.md`  
✅ Use Ansible Galaxy roles for standard services  

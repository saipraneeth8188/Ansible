# 04 - Variables and Facts in Ansible

## 🔹 Introduction

Variables and facts are key to making playbooks dynamic and reusable. You can define variables in various places, while facts are discovered automatically from managed nodes.

---

## 🔹 Defining Variables

### 1. In Playbook (inline)
```yaml
- hosts: web
  vars:
    pkg: httpd
  tasks:
    - name: Install package
      yum:
        name: "{{ pkg }}"
        state: present
```

### 2. In Inventory
```ini
[web]
web1 ansible_user=ec2-user myport=8080

[web:vars]
pkg=nginx
```

### 3. Using `vars_files`
```yaml
- hosts: all
  vars_files:
    - vars.yml
```

### 4. Using `vars_prompt`
```yaml
- hosts: localhost
  vars_prompt:
    - name: user_input
      prompt: "Enter your value"
      private: no
```

---

## 🔹 Facts in Ansible

Facts are data collected about the target system using the `setup` module.

### View all facts:
```bash
ansible all -m setup
```

### Filter facts:
```bash
ansible all -m setup -a 'filter=ansible_distribution'
```

### Disable fact gathering:
```yaml
- hosts: all
  gather_facts: no
```

---

## 🔹 Registering Variables

You can store output of a task into a variable using `register`.

```yaml
- name: Get uptime
  command: uptime
  register: result

- debug:
    var: result.stdout
```

---

## 🔹 Variable Precedence (High → Low)

1. Extra vars `-e`
2. Task-level vars
3. Block vars
4. Role vars
5. Play vars
6. Inventory group vars
7. Inventory host vars
8. Facts
9. Default vars

Use wisely to avoid confusion.

---

## 🔹 Variable Scopes

| Scope        | Defined In                             |
|--------------|----------------------------------------|
| Global       | `ansible.cfg`, command line            |
| Play         | Inside playbooks                       |
| Host/Group   | Inventory files                        |
| Role         | roles/role_name/vars/main.yml          |

---

## 🔹 `set_fact`

Used to define runtime variables dynamically:

```yaml
- set_fact:
    final_path: "/opt/{{ filename }}"
```

Useful for condition-based variable setting.

---

## 🔹 Jinja2 Templating

All variables use `{{ }}` syntax.

You can use filters:

```yaml
{{ somevar | lower }}
{{ somevar | upper }}
{{ somevar | default('value') }}
```

---

## Best Practices

✅ Use group_vars and host_vars directories  
✅ Use `set_fact` for dynamic decisions  
✅ Use `register` to capture output  
✅ Avoid naming conflicts and follow variable naming standards  

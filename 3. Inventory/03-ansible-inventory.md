# 03 - Ansible Inventory Deep Dive

Ansible Inventory is a file where you define the **hosts** (nodes) that Ansible will manage. It supports **static** and **dynamic** formats and plays a central role in organizing and targeting your infrastructure.

---

## 📁 Types of Inventory

### 1. Static Inventory
- Defined in an INI or YAML file.
- Most common and simple to use.

### 2. Dynamic Inventory
- Uses external scripts/plugins to pull live information (e.g., AWS EC2, GCP, Azure).
- Example: AWS dynamic inventory using `ec2.py` plugin.

---

## 🗂️ INI Inventory Format (Static)

```ini
[web]
192.168.1.10
192.168.1.11

[db]
192.168.1.20

[all:vars]
ansible_user=ec2-user
ansible_ssh_private_key_file=~/.ssh/id_rsa
```

✅ **Group Names**: `[web]`, `[db]`  
✅ **Host Entries**: IP or hostname  
✅ **Group Variables**: `[all:vars]` applies to all

---

## 📁 YAML Inventory Format

```yaml
all:
  hosts:
    localhost:
  children:
    web:
      hosts:
        192.168.1.10:
        192.168.1.11:
    db:
      hosts:
        192.168.1.20:
  vars:
    ansible_user: ec2-user
    ansible_ssh_private_key_file: ~/.ssh/id_rsa
```

---

## 🔄 Dynamic Inventory

Use when your infrastructure changes frequently (e.g., autoscaling EC2).  
Example (AWS EC2):

```bash
ansible-inventory -i aws_ec2.yaml --graph
```

You can write your own dynamic inventory script or use plugins like:

```yaml
plugin: aws_ec2
regions:
  - us-east-1
filters:
  tag:Role: webserver
```

Enable in `ansible.cfg`:

```ini
[inventory]
enable_plugins = host_list, script, yaml, ini, auto, aws_ec2
```

---

## 🧠 Best Practices

- Use YAML format for complex hierarchies.
- Group similar roles (e.g., `[app]`, `[backend]`, `[frontend]`).
- Prefer dynamic inventory for cloud-native setups.
- Centralize variables using `group_vars/` or `host_vars/` folders.

---

## 🧾 Inventory Testing

```bash
ansible all -i inventory.ini -m ping
```

Outputs:

```
192.168.1.10 | SUCCESS => pong
192.168.1.11 | SUCCESS => pong
192.168.1.20 | SUCCESS => pong
```

---

## 📌 Summary

| Concept            | Description                                      |
|--------------------|--------------------------------------------------|
| Static Inventory   | Defined in `.ini` or `.yaml`                     |
| Dynamic Inventory  | Real-time inventory from AWS, GCP, etc.          |
| Groups             | Logical units of hosts                           |
| Variables          | Assigned per host/group/all                      |
| Inventory Test     | `ansible all -m ping`                            |
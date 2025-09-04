# 03 - Inventory in Ansible

## What is an Inventory?

Ansible Inventory is a file (usually `hosts` or `inventory`) that defines the list of target machines to be managed. It tells Ansible which machines to communicate with and how to group them.

---

## Types of Inventory

### 1. Static Inventory
- Simple INI-style file.
- Hosts and groups defined manually.

**Example:**
```ini
[web]
192.168.1.10
192.168.1.11

[db]
db1.example.com ansible_user=admin
```

### 2. Dynamic Inventory
- Generated dynamically from cloud providers (AWS, GCP, etc.)
- Uses scripts or plugins.
- Supports scaling and dynamic environments.

---

## Inventory File Locations
- Default: `/etc/ansible/hosts`
- Custom inventory path can be passed using `-i` flag:
  ```bash
  ansible all -m ping -i custom_inventory
  ```

---

## Host Patterns
You can run commands on groups, individual hosts, or combinations.

| Pattern              | Description                           |
|----------------------|---------------------------------------|
| `all`               | All hosts in inventory                 |
| `web`               | Group named `web`                     |
| `web:db`            | Hosts in web OR db                    |
| `web:&db`           | Hosts common to both web AND db       |
| `web:!db`           | Hosts in web but NOT in db            |

---

## Grouping Hosts

You can nest groups and define children:

```ini
[app]
app1.example.com
app2.example.com

[db]
db1.example.com

[prod:children]
app
db
```

---

## Using Variables in Inventory

### Host-level Variables:
```ini
web1 ansible_host=192.168.0.1 ansible_user=ec2-user
```

### Group-level Variables:
```ini
[web]
web1
web2

[web:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=~/.ssh/id_rsa
```

---

## Inventory and Ansible Ad-hoc Commands

```bash
ansible all -m ping
ansible web -m shell -a "uptime"
```

You can filter hosts using `--limit`:
```bash
ansible all -m ping --limit "web1"
```

---

## Best Practices
- Use group variables for DRY code.
- Prefer YAML inventory for complex scenarios.
- Use dynamic inventory for cloud-native infrastructure.

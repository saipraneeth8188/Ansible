# 06 - Playbook Basics

## 🔹 What is a Playbook?

A playbook is a YAML file containing instructions for Ansible to execute on managed nodes. It defines a list of plays, each targeting a group of hosts and executing tasks.

---

## 🔹 Anatomy of a Playbook

```yaml
- name: Install and start Apache
  hosts: web
  become: yes
  tasks:
    - name: Install Apache
      yum:
        name: httpd
        state: present

    - name: Start Apache
      service:
        name: httpd
        state: started
        enabled: yes
```

---

## 🔹 Important Keywords

| Keyword       | Description                                |
|---------------|--------------------------------------------|
| `hosts`       | Target group from inventory                |
| `become`      | Enables privilege escalation (sudo)        |
| `tasks`       | List of actions to perform                 |
| `vars`        | Inline variables                           |
| `handlers`    | Triggered when a task reports `changed`    |
| `gather_facts`| Enables/disables fact collection           |

---

## 🔹 Ad-Hoc vs Playbook

- Ad-hoc: One-liner tasks (`ansible all -m ping`)
- Playbook: Structured, reusable, version-controlled

---

## 🔹 Using Modules in Playbooks

Playbooks use modules like `yum`, `copy`, `template`, etc.

```yaml
- name: Copy a file
  copy:
    src: file.txt
    dest: /tmp/file.txt
```

---

## 🔹 Using Loops

```yaml
- name: Install multiple packages
  yum:
    name: "{{ item }}"
    state: present
  loop:
    - git
    - wget
```

---

## 🔹 Using Handlers

```yaml
- name: Restart Apache
  service:
    name: httpd
    state: restarted
  when: ansible_os_family == "RedHat"
```

Handlers are triggered using `notify`.

---

## 🔹 Tags in Playbooks

```yaml
- name: Install Apache
  yum:
    name: httpd
    state: present
  tags:
    - apache
```

Run using:
```bash
ansible-playbook site.yml --tags "apache"
```

---

## 🔹 Running a Playbook

```bash
ansible-playbook playbook.yml
```

Add `-v`, `-vv`, or `-vvv` for verbose output.

---

## 🔹 Check Mode and Diff Mode

- Check mode:
  ```bash
  ansible-playbook play.yml --check
  ```
- Diff mode (shows file changes):
  ```bash
  ansible-playbook play.yml --diff
  ```

---

## 🔹 Best Practices

✅ One role per play  
✅ Use `become` for privilege tasks  
✅ Name every task  
✅ Use handlers for idempotent services  
✅ Use variables for flexibility  

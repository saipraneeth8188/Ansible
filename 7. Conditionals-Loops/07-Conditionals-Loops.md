# 07 - Conditionals and Loops in Ansible

## `when` Condition

```yaml
- name: Install package on CentOS
  yum:
    name: httpd
    state: present
  when: ansible_facts['os_family'] == "RedHat"
```

## `register` and `debug`

```yaml
- name: Check uptime
  command: uptime
  register: uptime_output

- debug:
    var: uptime_output.stdout
```

## Loops

```yaml
- name: Create multiple users
  user:
    name: "{{ item }}"
    state: present
  loop:
    - user1
    - user2
```
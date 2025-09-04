# 11. Error Handling and Debugging in Ansible

## Overview
Error handling is critical for managing unpredictable scenarios during automation. Ansible provides flexible control mechanisms to gracefully handle task failures, debug issues, and continue or stop playbook execution based on conditions.

---

## 1. `ignore_errors: yes`
This directive allows a playbook to continue execution even if a task fails.

```yaml
- name: Ignore error and continue
  command: /bin/false
  ignore_errors: yes
```

---

## 2. `failed_when` Condition
You can override Ansible’s default failure detection logic.

```yaml
- name: Override failure condition
  command: cat /tmp/test.txt
  register: output
  failed_when: "'No such file' in output.stderr"
```

---

## 3. `block`, `rescue`, `always`
These keywords group tasks together for structured error recovery.

```yaml
- name: Use block and rescue
  hosts: localhost
  tasks:
    - block:
        - name: Try to install package
          yum:
            name: httpd
            state: present
      rescue:
        - name: Print rescue message
          debug:
            msg: "Failed to install httpd!"
      always:
        - name: Always run this
          debug:
            msg: "Cleanup or logging task"
```

---

## 4. `any_errors_fatal: true`
Use this in play scope to fail the entire play if any task fails on any host.

```yaml
- name: Ensure all hosts must succeed
  hosts: all
  any_errors_fatal: true
```

---

## 5. Debugging with `debug` and `assert`

### `debug` Module
Used to print variable contents or messages for visibility.

```yaml
- debug:
    var: my_variable

- debug:
    msg: "Task completed successfully"
```

### `assert` Module
Used to validate conditions during playbook runs.

```yaml
- name: Assert OS is RHEL
  assert:
    that:
      - ansible_distribution == "RedHat"
```

---

## Summary

| Technique            | Description |
|----------------------|-------------|
| `ignore_errors`      | Skip failed task and continue |
| `failed_when`        | Custom failure condition |
| `block/rescue`       | Group tasks with error recovery |
| `any_errors_fatal`   | Fail play if one host fails |
| `debug` / `assert`   | Print/log/check variables and conditions |

Error handling and debugging are key for building resilient playbooks and understanding task outcomes.
# 08 - Handlers in Ansible

Handlers are tasks triggered only when notified.

## Example

```yaml
- name: Install Apache
  yum:
    name: httpd
    state: present
  notify: restart apache

handlers:
  - name: restart apache
    service:
      name: httpd
      state: restarted
```
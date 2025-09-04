# 06 - Playbook Basics

A playbook is a YAML file containing tasks to automate actions.

## Example Playbook

```yaml
- name: Install and configure Apache
  hosts: all
  become: yes
  tasks:
    - name: Install Apache
      yum:
        name: httpd
        state: present

    - name: Copy index.html
      copy:
        src: files/index.html
        dest: /var/www/html/index.html
```
Run the playbook using:
```bash
ansible-playbook playbook.yml
```
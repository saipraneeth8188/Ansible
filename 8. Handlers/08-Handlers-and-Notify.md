# 08 - Handlers and Notify

## 🔹 What are Handlers?

Handlers are special tasks in Ansible triggered only when notified by another task that results in a change. Ideal for restarting services, reloading config, etc.

---

## 🔹 Basic Usage of Handlers

```yaml
- name: Install Apache
  yum:
    name: httpd
    state: latest
  notify: restart apache

handlers:
  - name: restart apache
    service:
      name: httpd
      state: restarted
```

---

## 🔹 When Handlers Run

Handlers run **after all tasks** in a play are complete.

---

## 🔹 Multiple Notifications

```yaml
- name: Update config
  copy:
    src: httpd.conf
    dest: /etc/httpd/conf/httpd.conf
  notify:
    - restart apache
    - reload firewall
```

---

## 🔹 Defining Multiple Handlers

```yaml
handlers:
  - name: restart apache
    service:
      name: httpd
      state: restarted

  - name: reload firewall
    service:
      name: firewalld
      state: reloaded
```

---

## 🔹 Handlers with Tags

You can tag handlers:

```yaml
handlers:
  - name: restart nginx
    service:
      name: nginx
      state: restarted
    tags:
      - restart
```

Then run with:

```bash
ansible-playbook site.yml --tags "restart"
```

---

## 🔹 Using `listen` in Roles

Multiple tasks can notify the same handler using `listen`:

```yaml
- name: Install package
  yum:
    name: nginx
    state: present
  notify: restart webserver

- name: Update config
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
  notify: restart webserver

handlers:
  - name: restart webserver
    listen: restart webserver
    service:
      name: nginx
      state: restarted
```

---

## 🔹 `meta: flush_handlers`

Runs handlers immediately instead of waiting:

```yaml
- meta: flush_handlers
```

---

## 🔹 Best Practices

✅ Use handlers for idempotent service restarts  
✅ Keep handler names descriptive  
✅ Use `listen` for role-based modular handlers  
✅ Use `meta: flush_handlers` for early execution  

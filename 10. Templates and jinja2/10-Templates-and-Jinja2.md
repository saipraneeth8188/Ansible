# 10 - Templates and Jinja2

## 🔹 What is a Template?

Templates are configuration files created using Jinja2 syntax. These are `.j2` files processed by Ansible to generate real config files with dynamic content.

---

## 🔹 Example: Jinja2 Template

`index.html.j2`:

```html
<html>
  <head><title>{{ title }}</title></head>
  <body>
    <h1>Welcome {{ user }}</h1>
  </body>
</html>
```

---

## 🔹 Using Templates in a Playbook

```yaml
- name: Deploy HTML template
  template:
    src: index.html.j2
    dest: /var/www/html/index.html
```

---

## 🔹 Variables in Templates

Variables can be passed from:

- Inventory
- `vars:` section
- Facts
- Registered values

```yaml
vars:
  user: Sai
  title: DevOps Training
```

---

## 🔹 Conditionals in Templates

```jinja2
{% if user == "admin" %}
  Welcome, admin!
{% else %}
  Welcome, user!
{% endif %}
```

---

## 🔹 Loops in Templates

```jinja2
<ul>
{% for item in services %}
  <li>{{ item }}</li>
{% endfor %}
</ul>
```

---

## 🔹 Filters in Jinja2

```jinja2
{{ 'abc' | upper }}         # Output: ABC
{{ mylist | length }}       # Output: Length of list
{{ num | int }}             # Convert to integer
```

---

## 🔹 Using Facts in Templates

```jinja2
Server hostname: {{ ansible_hostname }}
OS Family: {{ ansible_os_family }}
```

---

## 🔹 Best Practices

✅ Keep `.j2` files in `templates/` directory inside roles  
✅ Use meaningful variable names  
✅ Avoid hardcoded values  
✅ Test templates using `--check` mode  

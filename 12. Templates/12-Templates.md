# 12. Templates in Ansible

## What Are Templates?
Templates in Ansible are files written in Jinja2 format (`.j2`) that allow dynamic configuration file generation using variables.

## Use Case Example
Imagine generating a custom `index.html` with hostname and date.

## Sample Template - `index.html.j2`
```html
<html>
  <body>
    <h1>Welcome to {{ inventory_hostname }}</h1>
    <p>Deployed on {{ ansible_date_time.date }}</p>
  </body>
</html>
```

## Playbook Task Example
```yaml
- name: Deploy index page
  template:
    src: index.html.j2
    dest: /var/www/html/index.html
```

## Template Location
Templates are typically stored in:
```
roles/
  myrole/
    templates/
      index.html.j2
```

---
Templates help you render configuration files dynamically using variables and facts.
# 07 - Looping and Iterating in Ansible

## 🔹 Why Loops?

Loops let you execute a task multiple times with different values. Ansible supports multiple looping constructs.

---

## 🔹 Basic `loop`

```yaml
- name: Install multiple packages
  yum:
    name: "{{ item }}"
    state: present
  loop:
    - httpd
    - git
    - wget
```

---

## 🔹 `with_items` (older style)

```yaml
- name: Create users
  user:
    name: "{{ item }}"
    state: present
  with_items:
    - alice
    - bob
```

`loop` is preferred over `with_items`.

---

## 🔹 `with_dict`

Iterating over key-value pairs:

```yaml
- name: Create multiple files
  copy:
    content: "{{ item.value }}"
    dest: "/tmp/{{ item.key }}"
  with_dict:
    file1.txt: "This is file 1"
    file2.txt: "This is file 2"
```

---

## 🔹 `with_nested`

Iterating through combinations:

```yaml
- name: Install packages on OS types
  debug:
    msg: "Installing {{ item[0] }} on {{ item[1] }}"
  with_nested:
    - [ 'httpd', 'nginx' ]
    - [ 'RedHat', 'Debian' ]
```

---

## 🔹 `loop_control`

Customize loop behavior:

```yaml
- name: Print items with index
  debug:
    msg: "{{ item }}"
  loop:
    - one
    - two
    - three
  loop_control:
    index_var: idx
```

---

## 🔹 `pause` with loop

```yaml
- name: Wait between tasks
  pause:
    seconds: 5
  loop:
    - 1
    - 2
    - 3
```

---

## 🔹 Registering Loop Output

```yaml
- name: Ping multiple hosts
  shell: "ping -c1 {{ item }}"
  loop:
    - google.com
    - yahoo.com
  register: ping_out

- debug:
    var: ping_out.results
```

Each loop result is stored in `.results`.

---

## 🔹 Conditional with Loops

```yaml
- name: Skip nginx
  yum:
    name: "{{ item }}"
    state: present
  loop:
    - httpd
    - nginx
  when: item != "nginx"
```

---

## 🔹 Best Practices

✅ Use `loop` instead of legacy `with_items`  
✅ Use `loop_control` to enhance readability  
✅ Register and use loop outputs effectively  
✅ Use `when` to control loop iterations  

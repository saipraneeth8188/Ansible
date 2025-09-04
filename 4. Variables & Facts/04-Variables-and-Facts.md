# 04 - Variables and Facts in Ansible

This section summarizes the deep handling of variables and facts from RH Ansible PDF notes.

---

## What are Variables in Ansible?

Variables let you customize your playbooks and are used to store values (e.g., IPs, users, paths) for dynamic automation.

---

## Ways to Define Variables

| Method | Description |
|--------|-------------|
| Inventory | Defined per host/group inside `inventory` |
| `vars` block | Directly inside the playbook |
| `vars_files` | External YAML variable files |
| `host_vars` | Folder containing per-host YAML files |
| `group_vars` | Folder containing per-group YAML files |
| `set_fact` | Dynamic variable assignment during play |
| Extra Vars | `-e key=value` at runtime |

---

## Facts in Ansible

Facts are system properties collected from remote nodes using the `setup` module.

**Examples:**
- `ansible_hostname`
- `ansible_distribution`
- `ansible_processor_cores`

Use:
```yaml
- debug:
    var: ansible_facts['distribution']
```

Disable fact gathering:
```yaml
gather_facts: no
```

---

## Precedence Order (Highest to Lowest)

1. Extra Vars `-e`
2. Task `set_fact`
3. Block `vars`
4. Role defaults

---

## Good Practices

- Separate variables cleanly
- Use `group_vars/` and `host_vars/` folders for clarity
- Use `ansible_facts['key']` for facts explicitly

---

## Debugging Variables

Use the `debug` module to print variable values:
```yaml
- debug:
    var: my_var
```
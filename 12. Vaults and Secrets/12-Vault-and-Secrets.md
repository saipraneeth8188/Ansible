# 12. Ansible Vault and Secret Management

## Overview
Ansible Vault allows you to encrypt secrets such as API keys, passwords, and private variables inside YAML files or even full playbooks. This is crucial for production-grade infrastructure automation.

---

## 1. Creating Encrypted Files

```bash
ansible-vault create secret.yml
```

You’ll be prompted to enter a password. After creation, it opens in your default editor.

---

## 2. Editing Encrypted Files

```bash
ansible-vault edit secret.yml
```

---

## 3. Encrypting Existing Files

```bash
ansible-vault encrypt existing_file.yml
```

---

## 4. Decrypting Files

```bash
ansible-vault decrypt secret.yml
```

---

## 5. Viewing Encrypted Files (Read-only)

```bash
ansible-vault view secret.yml
```

---

## 6. Using Vault in Playbooks

```yaml
vars_files:
  - secret.yml
```

The file will be decrypted at runtime if you pass the password or vault ID.

---

## 7. Running Encrypted Playbooks

```bash
ansible-playbook secure-playbook.yml --ask-vault-pass
```

Or using a password file:

```bash
ansible-playbook secure-playbook.yml --vault-password-file ~/.vault_pass.txt
```

---

## 8. Encrypting Specific Variables Inline

```yaml
db_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          3939323537653663626537343635...
```

---

## 9. Re-keying Vault

To change the password of an encrypted file:

```bash
ansible-vault rekey secret.yml
```

---

## 10. Best Practices

- Use `.vault_pass.txt` and restrict its permission (`chmod 600`)
- Do not commit unencrypted secrets to Git
- Use `ansible-vault encrypt_string` for one-liner secrets
- Keep vault IDs for environment separation (`dev`, `prod`)

---

## Summary

| Command                            | Use Case |
|------------------------------------|----------|
| `create` / `edit` / `view`         | Manage secret files |
| `encrypt` / `decrypt` / `rekey`    | Encrypt/decrypt full YAMLs |
| `encrypt_string`                   | Inline secret encryption |
| `--vault-password-file`            | Automated decryption in CI/CD |
# My nvim dev environment setup
This is WIP
- nvim
- php
- ocaml

## Prerequisites

### Vault Password Setup
This repository uses Ansible Vault to encrypt sensitive data (e.g., Intelephense activation key). Before running the playbooks, you need to set up the vault password file.

**On the target machine, create the vault password file:**
```bash
# Create the vault password file in your home directory
# Replace YOUR_VAULT_PASSWORD with your actual vault password
echo 'YOUR_VAULT_PASSWORD' > ~/.vault_pass

# Secure the file (readable only by you)
chmod 600 ~/.vault_pass
```

**Note:** Keep your vault password secure and never commit it to version control.

## Usage

### Option 1: Using ansible-pull with vault password file
```bash
ansible-pull -U https://github.com/pasanec/local_ansible.git \
  -i localhost, \
  localhost.yml \
  --vault-password-file ~/.vault_pass \
  --extra-args "-e ansible_roles_path=$(pwd)/roles"
```

### Option 2: Using ansible.cfg with vault password file
If you've copied or cloned the repository with `ansible.cfg` that includes `vault_password_file = ~/.vault_pass`:
```bash
ansible-pull -U https://github.com/pasanec/local_ansible.git \
  -i localhost, \
  localhost.yml \
  --extra-args "-e ansible_roles_path=$(pwd)/roles"
```

### Option 3: Interactive password prompt
If you prefer to enter the vault password manually each time:
```bash
ansible-pull -U https://github.com/pasanec/local_ansible.git \
  -i localhost, \
  localhost.yml \
  --ask-vault-pass \
  --extra-args "-e ansible_roles_path=$(pwd)/roles"
```

## Testing Vault Decryption

To verify that vault decryption works correctly:
```bash
# Create a test playbook
cat > test_vault.yml << 'EOF'
---
- name: Test Vault Decryption
  hosts: localhost
  connection: local
  gather_facts: no
  vars_files:
    - roles/base/vars/main.yml
  tasks:
    - name: Verify vault works
      debug:
        msg: "✓ Vault OK! Key length: {{ intelephense_activation_key | length }}"
EOF

# Run the test with vault password file
ansible-playbook test_vault.yml --vault-password-file ~/.vault_pass

# Clean up
rm test_vault.yml
```

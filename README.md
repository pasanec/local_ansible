# My nvim dev environment setup
This is WIP
- nvim
- php
- ocaml
# Usage
```bash
ansible-pull -U https://github.com/pasanec/local_ansible.git \
  -i localhost, \
  localhost.yml \
  --extra-args "-e ansible_roles_path=$(pwd)/roles"
```

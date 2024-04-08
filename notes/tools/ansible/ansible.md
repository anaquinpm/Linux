# Ansible

## Configurations

```bash
## order of priotity
ANSIBLE_CONFIG="path/to/my/file.conf"  # environment variable
ansible.cfg     # current directory
~/.ansible.cfg  # home directory
/etc/ansible/ansible.cfg # global configuration
```

## Playbooks

It's a way of grouping tasks/processes that will be executed on several servers.

## Commands

```bash
apt install ansible
ansible --version
ansile --list-hosts <name_resourse_list|pattern>
ansible -m ping all
ansible-playbook </path/to/playbook_file.yml> [-K]    # -K ask for privilege scalation password
  [--check] -> dry-run to check our playbooks
  [--tags|--skip-tags] <name_tag> -> execute the tasks with the same name_tag
ansible -m setup <inventory_group|server_entry>   # variables|meta-data created during the interaction with a server
ansible-galaxy -> search how to use "ROLES"
ansible-vault create secret-variables.yml     # create encrypted data file
  ansible-vault edit secret-variables.yml
  ansible-playbook <app.yml> --ask-vault-pass # prompt for password
```

## References

- [Jinja2 template lenguage](https://jinja.palletsprojects.com/en/2.10.x/templates/)
- [Ansible Modules](https://docs.ansible.com/ansible/latest/collections/index_module.html)
- [Roles](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
  - [Ansible-galaxy](https://galaxy.ansible.com/ui/)
- [Vault - encrypt files](https://docs.ansible.com/ansible/latest/vault_guide/index.html)
- [Prompts - interactive inputs](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_prompts.html)

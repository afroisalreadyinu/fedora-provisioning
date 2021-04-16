# Fedora workstation provisioning plays for Ansible

The file `provision.yml` in this repo is a sample Ansible playbook to provision
a complete development workstation with developer tools, configuration, SSH
keys, desktop applications and keyboard configuration.

## Preparing secrets

Encrypt the secrets with ansible vault.

## Installing Ansible

Use the default one
```
dnf install ansible
```

After copying to target, run with:

```
ansible-galaxy collection install community.general ansible.posix
ansible-playbook --ask-become-pass --ask-vault-pass provision.yml --extra-vars compile=true
```

## Missing

- I'd like to sign in to my Firefox profile using Ansible, but looks like it's
  too much work.

- Same for Chrome

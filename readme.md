# Ansible playbook for provisioning Fedora workstation

The file `provision.yml` in this repo is a sample Ansible playbook to provision
a complete development workstation with developer tools, configuration, SSH
keys, desktop applications and keyboard configuration. It can be used to make a
fresh Fedora install ready for serious work within a couple of minutes.

## Preparing secrets

You need to clone this repository (or copy the files somewhere else), make it
private, and add your SSH files, encrypting the private one(s) with Ansible
Vault. The associated commands look like this:

```
cd files/
ansible-vault encrypt ~/.ssh/id_rsa --output id_rsa.vault
cp ~/.ssh/id_rsa.pub .
```

Vault will prompt you for an encryption password; make sure to pick something
you can remember later. The playbook includes steps for restoring AWS CLI
configuration from similar vault-encrypted secrets; instructions for encrypting
them are similar, but make sure that you use the same encryption password for
all of them.

## Running

On a new workstation, you will need to install Ansible and the `ansible.posix` collection:

```
sudo dnf install ansible
ansible-galaxy collection install community.general ansible.posix
```

Once these are installed, copy the contents of your provisioning repo, and run
the following command:

```
ansible-playbook --ask-become-pass --ask-vault-pass provision.yml
```

Ansible will ask for your password, and then the vault password you picked
earlier. Afterwards, the steps in the playbook will be executed. If there is an
error, you can fix it, and then run the whole playbook again. Ansible will skip
any already-executed steps, as most Ansible modules are
[idempotent](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html#desired-state-and-idempotency).

## Missing

- I'd like to sign in to my Firefox profile using Ansible, but looks like it's
  too much work. Same for Chrome.

- ~~Keyboard customizations (mapping caps lock to escape and removing super-s
  keybinding) are done using the `gsettings` command, causing them to be
  executed every time.~~ Fixed by using the `dconf` module; thanks to
  [I_am_a_human_male](https://www.reddit.com/r/Fedora/comments/ms0tez/ansible_playbook_example_for_provisioning_a/gupogj5).

- Somehow neither the `dnf` nor `yum_repository` Ansible modules could deal with
  with the Hashicorp YUM repo. Any tips would be appreciated.

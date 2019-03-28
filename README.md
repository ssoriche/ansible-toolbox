# ansible-toolbox

Collection of roles/playbooks to make day to day operations with multiple
systems easier.

## Playbooks

### run_cmd.yml

Executes the contents of the `command` variable, and captures output per host
into the `logs/` directory.

Examples:

Run `ls -al` on all hosts in the `metacpan` inventory file.

```shell
ansible-playbook \
  -i inventories/samples \
  --extra-vars "command='ls -al'" \
  --ask-sudo-pass --become \
  playbooks/run_cmd.yml
```

Look for a cron entry that contains the phrase `GMC`.

```shell
ansible-playbook \
  -i inventories/samples \
  --extra-vars "command='grep -R GMC /etc/cron.d'" \
  --ask-sudo-pass --become \
  playbooks/run_cmd.yml
```

```shell
ansible-playbook \
  -i inventories/samples \
  --extra-vars "command='grep -R GMC /var/spool/cron'" \
  --ask-sudo-pass --become \
  playbooks/run_cmd.yml
```

Issue `curl` command on each server to test localhost response

```shell
ansible-playbook \
  -i inventories/samples \
  --extra-vars "command='curl -XGET \"http://localhost:5000/v1/search/web?q=Mojo&collapsed=blah\"'" \
  playbooks/run_cmd.yml
```

### ssh_client.yml

Deploy ssh keys

```shell
ansible-playbook \
  --ssh-common-args="-o PreferredAuthentications=password -o PubkeyAuthentication=no" \
  --ask-pass \
  -i inventories/samples \
  -u new_project_user \
  playbooks/setup_ssh_keys.yml
```

The above play relies on the `inventories/samples/group_vars/all.yml` to
contain the following variables:

```
---

ansible_user: new_project_user
ssh_username: new_project_user
ssh_key: new_project_key
```

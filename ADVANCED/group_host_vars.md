# Group and host vars

Ansible is a variable driven tool, just as Terraform for example. Variables are used to store information, and to make decisions.

Group vars and host vars are a way to store variables for a group of hosts, or a single host.

Think of this scructure:

Let's define some variable applicable to all hosts:

`group_vars/all/backup.yml`:
```yaml
backup_interval: 3600
```

Now let's set a path to backup for the webservers:

`group_vars/webservers/backup.yml`
```yaml
backup_paths:
  - /var/www/html
```

And backup something else for the databaseservers:

`group_vars/databaseservers/backup.yml`
```yaml
backup_paths:
  - /var/lib/mysql
```

As you can see, you can set a value for a variable based on the group the host is in.

The same works for host vars, but then for a single host. The group_vars are mostly more applicable than host_vars, but you can use both.

More specific variables will override the more general ones. The different levels and priority is described in the [Ansible documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html#understanding-variable-precedence).

## Assignment

> Note: This assignment requires the [previous assignment on inventory](inventory) to be completed.

1. Create a `group_vars` directory.
2. Create a `group_vars/all/ntp.yml` file.
3. Set the `ntp_server` variable to `0.pool.ntp.org` for all hosts.
4. Create a `group_vars/switzerland/ntp.yml` file.
5. Set the `ntp_server` variable to `0.ch.pool.ntp.org` for all hosts in the `switzerland` group.
6. Set the `ntp_server` variable to `0.nl.pool.ntp.org` for all hosts in the `netherlands` group.
 
 ### Verify

Run `ansible netherlands -m debug -a "var=ntp_server"`. You should see this output:

```text
node-1 | SUCCESS => {
    "ntp_server": "0.nl.pool.ntp.org"
}
node-3 | SUCCESS => {
    "ntp_server": "0.nl.pool.ntp.org"
}
```

Run `ansible switzerland -m debug -a "var=ntp_server"`. You should see this output:

 ```text
 node-2 | SUCCESS => {
    "ntp_server": "0.ch.pool.ntp.org"
}
node-4 | SUCCESS => {
    "ntp_server": "0.ch.pool.ntp.org"
}
```

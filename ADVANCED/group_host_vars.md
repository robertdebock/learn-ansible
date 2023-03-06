# Group and host vars

Ansible is a variable driven tool. Variables are used to store information, and to make decisions.

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

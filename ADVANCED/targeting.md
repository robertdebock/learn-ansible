# Targeting

With Ansible you can "taget" (or select) hosts using a couple of patterns.

For example, here is a playbook snippet that will run on all hosts:

```yaml
- name: Do something on all hosts
  hosts: all

  tasks:
    - name: Say something
      debug:
        msg: "Hello world!"
```

As you can see, the `hosts` parameter is targeting `all` hosts.

There are a couple of patterns that are interesting:

- `all` - all hosts in the inventory.
- `webservers` - all hosts in the `webservers` group. (Or any other group that you have defined in the [inventory](inventory).)
- `node-1` - a single host, by name. There is actually no different in targeting a group and a single host, so pick you hostnames and groupnames carefully.
- `node-1:node-2` - multiple hosts, by name. You can use a colon (`:`) to separate multiple hosts.
- `node-*` - multiple hosts, by name. You can use a wildcard to select multiple hosts.
- `all:!node-1` - all hosts, except `node-1`. You can use an exclamation mark to exclude a host.
- `webservers:&production` - all hosts in the `webservers` and `production` group. You can use an ampersand to make a cross-selection of groups.

## Limiting

You can also restict the hosts that are targeted by using the `--limit` parameter. This is a reduction from the targeted pattern defined in `hosts` in the playbook.

> Note: The `--limit` parameter can be shortened to `-l`.

For example, this inventory:

```ini
[webservers]
node-1
node-2
node-3
node-4

[databaseservers]
node-5
node-6
node-7
node-8

[development]
node-1
node-5

[test]
node-2
node-6

[accepance]
node-3
node-7

[production]
node-4
node-8
```

With this playbook:

```yaml
- name: Do something on the webservers hosts
  hosts: webservers

  tasks:
    - name: Say something
      debug:
        msg: "Hello world!"
```

And this limit:

```shell
ansible-playbook playbook.yml --limit development
```

Would run on node-1 only.

> Note: Limiting on the command line, like above, has a disadvantage of not being stored in code.

## Assignment

1. Create an inventory with the following groups:

- `applicationservers`
- `databaseservers`
- `production`
- `development`

> Note: You need to tell Ansible (in `ansible.cfg`, under the `[defaults]` section, to use your inventory file as the inventory. The parameter is `inventory`.)

2. Add the following hosts:

- `node-1` (an production application server)
- `node-2` (an production database server)
- `node-3` (an development application server)
- `node-4` (an development database server)

4. Run the playbook with a limit on `development`.

> Note: The nodes are not reachable, so the task will fail. That's okay.

## Verify

You should see the playbook run on `node-3` only.

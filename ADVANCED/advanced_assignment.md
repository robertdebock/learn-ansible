# Advanced Assignment

This assignment takes just about all the knowledge you have gained so far and combines it into a single assignment.

## Assignment 0

We need to create some infrastructure first. In the explanation below we'll use Docker and docker-compose, but you can use any infrastructure you like. Eventually you'll need 4 hosts, 2 webservers and 2 databaseservers:

(The hostnames can vary a bit, the hostnames you will be using should end up in the Ansible inventory.)

| Hostname | Group           |
| -------- | --------------- |
| node-1   | webservers      |
| node-2   | webservers      |
| node-3   | databaseservers |
| node-4   | databaseservers |

Feel free to use the [infrastructure](infratructure) directory as a starting point.

All the assignments and examples will be based on this Docker setup.

## Assignment 1

Let's first setup Ansible without too much Ansible code, just the inventory and it's variable.

1. Create a directory for this assignement, for example `advanced_assignment`.
2. Create an `ansible.cfg` file in this directory.
3. Configure `ansible.cfg` to use the `inventory` directory as inventory.
4. Create an `inventory` directory.
5. Create an inventory file `hosts` in the `inventory` directory.
6. Create a group `webservers` in the inventory file.
7. Add 2 hosts to the `webservers` group: `node-1` and `node-2`, both using `ansible_connection=docker`.
8. Create a group `databaseservers` in the inventory file.
9. Add 2 hosts to the `databaseservers` group: `node-3` and `node-4`, both using `ansible_connection=local`.
10. Create a group `production` in the inventory file, add `node-1` and `node-3` to it.
11. Create a group `development` in the inventory file, add `node-2` and `node-4` to it.
12. Create a directory for the `group_vars`.
13. Create a file `group_vars/all/ntp.yml` in the `group_vars` directory.
14. Set the `ntp_server` variable to `0.pool.ntp.org` for all hosts.

## Verification of assignment 1

Once the above steps are completed, run these commands to verify if all is working as expected:

```shell
ansible -m ping all
```

The value `all` used in the above command can be changed for:

- `webservers`
- `databaseservers`
- `production`
- `development`

| Group             | Expected output                                     |
| ----------------- | --------------------------------------------------- |
| `all`             | 4 nodes: `node-1`, `node-2`, `node-3` and `node-4`. |
| `webservers`      | 2 nodes: `node-1` and `node-2`.                     |
| `databaseservers` | 2 nodes: `node-3` and `node-4`.                     |
| `production`      | 2 nodes: `node-1` and `node-3`.                     |
| `development`     | 2 nodes: `node-2` and `node-4`.                     |

Running this command:

```shell
ansible -m ansible.builtin.debug -a "msg={{ ntp_server }}" all
```

Should return `0.pool.ntp.org` for all nodes.

## Assignment 2

You should have assignment 1 ready before starting this assignment.

1. Create a playbook `playbook.yml` in the `advanced_assignment` directory.
2. Install these tools on all nodes: `sudo`, `tcpdump`, `vim`, `wget`.
3. Install Apache HTTPD (the package differs for each distribution) on all nodes in the `webservers` group.
4. Start and enable Apache HTTPD on all nodes in the `webservers` group.
5. Install MariaDB (the package differs for each distribution) on all nodes in the `databaseservers` group.
6. Start and enable MariaDB on all nodes in the `databaseservers` group.

To prevent a high load, set the `serial` parameter to 50% in the playbook.

## Verification of assignment 2

Once the above steps are completed, run these commands to verify if all is working as expected:

```shell
ansible-playbook playbook.yml
```

You should see:

- All nodes being managed.
- The installation of the packages being done.
- The starting of the services being done.

As a bonus; try to keep your playbooks or roles idempotent.

## Assignment 3

We're going to create a play for the NTP setup.

> Note: The name `ntp` is being used, but the package name, configuration file and service name change over different distributions.

| Distribution | Package name | Configuration file | Service name |
| ------------ | ------------ | ------------------ | ------------ |
| Ubuntu       | `ntp`        | `/etc/ntp.conf`    | `ntp`        |
| CentOS       | `chrony`     | `/etc/chrony.conf` | `chronyd`    |

1. Add a task in the `playbook.yml` for `all` nodes to install the package for NTP.
2. Add a task to configure the NTP server for all nodes in the `webservers` group to `0.ch.pool.ntp.org`. Make sure to restart NTP on changes made to the configuration file.
3. Start the NTP service.

Some hints on the configuration file: You can login to the nodes and `cat` a default NTP configuration, and place a modified version in the `templates` directory of your role.

## Verification of assignment 3

Run this Ansible command and check that nothing has changed.

> Note, please remember that different names of the package, configuration file and service.

```shell
ansible -m lineinfile -a "path=/etc/chrony.conf line='pool 0.pool.ntp.org iburst'" all --check
```

## Assignment 4

Modify the playbook to:

1. Place an `index.html` on the `webservers`.
2. Make sure the content of `index.html` read something like this: "Hello from MY_HOST_NAME." (Where `MY_HOST_NAME` is the variable hostname of the node.)

## Verification of assignment 4

Run this command: `curl http://localhost:80` and make sure "Hello from node-1." is returned.
Run this command: `curl http://localhost:81` and make sure "Hello from node-2." is returned.

## Assignment 5

We're going to play with error handling.

Append to the playbook, a task that:

- Displays a message, something like "Hello."
- Randomly fails. You'll need to use the [`random`](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/random_filter.html) filter.
- In case the job (randomly) failed, display a message, something like "Oh no, it failed.".

## Verification of assignment 5

When running the playbook a few times, you should see the play summary, where  `rescued` is `1`:

```text
node-1       : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0   
node-2       : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
node-3       : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
node-4       : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0    
```

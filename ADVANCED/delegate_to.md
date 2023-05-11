# delegate_to

You can run a task on a specific host using `delegate_to`.

```yaml
- name: Do something on the first host
  ansible.builtin.command:
    cmd: ls
  delegate_to: "{{ groups['all'][0] }}"
```

The above example will run the `ls` command on the first host in the `all` group.

You may want to use this pattern to run something on another node. An example can be to register a system to a CMDB:

```yaml
- name: Install stuff on a node
  ansible.builtin.package:
    name: "stuff"

- name: Register this node to the CMDB
  ansible.builtin.uri:
    url: "https://cmdb.example.com/register"
    method: POST
    body: "{{ inventory_hostname }}"
  delegate_to: localhost
```

## Assignment

Create a playbook that stores the hostname of the current node in a file on `localhost`; /tmp/hosts.txt

## Solution

A [hacky attempt](https://github.com/robertdebock/learn-ansible-solutions/blob/master/delegate_to/) that works.

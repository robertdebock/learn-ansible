# Serial

You can request Ansible to run a few tasks at a time. This can be useful to do a "gradual deployment", where some nodes will be modified first, and others later.

```yaml
- name: Do something on the webservers hosts
  hosts: webservers
  serial: 2

  tasks:
    - name: Say something
      debug:
        msg: "Hello world!"
    
    - name: Say something else
      debug:
        msg: "Hello world again!"
```

If the above playbook is run on 4 nodes, it will run on 2 nodes at a time in this order:

1. node-1 "Hello world!
2. node-2 "Hello world!
3. node-1 "Hello world again!
4. node-2 "Hello world again!
5. node-3 "Hello world!
6. node-4 "Hello world!
7. node-3 "Hello world again!
8. node-4 "Hello world again!

There are some tricks to `serial`:

| Serial                   | Description                                                                        |
| ------------------------ | ---------------------------------------------------------------------------------- |
| `1`                      | Run on all nodes at once.                                                          |
| `10%`                    | Run on 10% of the nodes at once.                                                   |
| `[1, 2, 3]`              | Run on 1 node, then 2 nodes, then 3 nodes.                                         |
| `["10%", "20%", "100%"]` | Run on 10% of the nodes, then 20% of the nodes, then all (remaining) of the nodes. |

## Assignment

Write a playbook that runs on the first 10%, next, 3 nodes, finally 100% of the nodes. Add a simple task that displays a message.

Make sure the inventory contains at least 10 nodes. You can use a pattern in the inventory: `node-[1:10] ansible_connection=local`.

## Solution

Have a look at [this solution](https://github.com/robertdebock/learn-ansible-solutions/blob/master/serial/).

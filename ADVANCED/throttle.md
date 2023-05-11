# Throttle

Besides [`serial`](serial), you can use `thottle` to limit the number of nodes that are targeted at the same time for a task or block.

```yaml
- name: Do something on the webservers hosts
  hosts: all

  tasks:
    - name: Say something
      debug:
        msg: "Hello world!"

    - name: Simulate an intensive job
      ansible.builtin.pause:
        seconds: 3
      throttle: 1

    - name: Say something else
      debug:
        msg: "Hello world again!"
```

This can be used when a job consumes resources which could break some central component. Examples could be:

- Update packages, preventing a bottleneck on a repository-server.
- Do a backup to a central backup-server.
- Run a job that uses a central database, preventing a bottleneck on the database-server.

## Assignment

Write a playbook that has a couple of tasks:

1. Print "Hello World!".
2. Use the [`find`](https://docs.ansible.com/ans`ible/latest/collections/ansible/builtin/find_module.html) module to find all files on the managed nodes in the `/opt` directory. This is a filesystem-intensive task. Using `throttle` helps to prevent disk io saturation.

## Solution

This assignment is a little more open/difficult. Have a quick look [here](https://github.com/robertdebock/learn-ansible-solutions/blob/master/throttle/) for a possible solution.

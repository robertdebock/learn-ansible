# Run once

The `run_once` parameter can be set on any task. It will ensure that a task is run only once, even when multiple hosts are in scope.

For example, if you are running a playbook on multiple nodes, but you need to run one specific task just once on a host, you can use `run_once`. This can be used to select a single host to run a task on, for example to run a task on one node of a cluster.

```yaml
- name: Install a cluster
  hosts: cluster_a
  become: yes
  
  tasks:
    - name: Check something on one node
      ansible.builtin.assert:
        that:
          - ansible_distribution == "Ubuntu"
      run_once: true
    
    - name: Do something to all nodes
      ansible.builtin.debug:
        msg: "I'm doing something to all nodes."
```

Use-cases for `run_once`:

- Initialize a cluster.
- Create a directory on the [delegated host](delegate_to) `localhost`.
- Interact with an API after installation of the API software on all nodes.

## Assignment

Write a playbook that:

1. Creates a directory on the `localhost` once.
2. [Fetch](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/fetch_module.html) "install.log" (or so) from the managed nodes to the controller node.

## Solution

A [possible solution here](https://github.com/robertdebock/learn-ansible-solutions/blob/master/run_once/).

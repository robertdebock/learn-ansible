# Order

The playbook parameter `order` can be used to control the order hosts running tasks.

```yaml
- name: Do something to hosts, in a random order
  hosts: all
  order: shuffle

  tasks:
    - ansible.builtin.debug:
        msg: "Hello world!"
```

This can be used to make the order of hosts more predictable, or more random. For example, the default `order` is `inventory`, which would mean one hosts is always first. This may result in the first host being picked all the time, so that host may respond differntly than the other hosts over time. The `order` value `shuffle` would mitigate this issue.

## Assignment

1. Write a playbook that runs on all hosts, in a random order.

2. Play with these `order` parameters:

- `reverse_inventory`
- `sorted`
- `reverse_sorted`

## Solution

Have a look at [this random order](https://github.com/robertdebock/learn-ansible-solutions/blob/master/order/)

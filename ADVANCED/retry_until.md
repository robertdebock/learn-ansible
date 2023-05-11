# Retry & until

You can retry a task a number of times, or until a condition is met.

One example is to retry tasks that depend on something external, like installing a package that depends on a repository.

```yaml
---
- name: Retry something
  hosts: all

  tasks:
    - name: Install some package.
      ansible.builtin.package:
        name: some_package
      register: result
      until: result is not failed
      retries: 2
      delay: 10
```

> The `retries` is the number of attempts after the first one. So a `retries: 2` means the job is attempted 3 times.

## Assignment

Write a playbook that:

1. Uses the [`stat`](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/stat_module.html) to check if a file exists.
2. Retries 9 times, with a delay of 2 seconds.

[This solution](https://github.com/robertdebock/learn-ansible-solutions/blob/master/retry_until/) works.

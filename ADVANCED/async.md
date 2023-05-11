# Async

The "async" task parameter can be used to push a command to the background and continue the play or task-list. This is useful for long-running operations, like installing a large application or waiting for a reboot to complete.

This parameter "async" can also be used to run a long running job and keep the connection alive.

```yaml
- name: Do something that takes a long time
  ansible.builtin.package:
    name: "*"
    state: latest
  async: 3600
  poll: 0
  register: long_running_task

- name: Do something in between
  ansible.builtin.command:
    cmd: "echo 'Hello world!'"

- name: Check if the long running task is done
  ansible.builtin.async_status:
    jid: "{{ item.ansible_job_id }}"
  loop: "{{ long_running_task }}"
  register: job_status
  until: job_status.finished
  retries: 359
  delay: 10
```

In the example above, the command will be pushed to the background and Ansible will continue with the next task. Ansible will check every 10 seconds if the command has completed. If the command has not completed after 3600 seconds, Ansible will fail the task.

## Assignment

Write a playbook that has a couple of tasks:

1. Sleep for 10 seconds. Use `async` to push this job to the background.
2. Show "Hello World!".
3. Check if the first sleep task is done.

## Solution

Have a look at [this solution](https://github.com/robertdebock/learn-ansible-solutions/blob/master/async/).

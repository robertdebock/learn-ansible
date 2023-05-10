# Registering Results

A task produces some output. That output can be used in a later task.

Let's have a look at the results that Ansible produces for a task.

```yaml
- name: Play with results
  hosts: all

  tasks:
    - name: Run some task
      ansible.builtin.debug:
        msg: "Hello world!"
      register: output

    - name: Show the output
      debug:
        msg: "{{ output }}"
```

The output will be:

```text
TASK [Run some task] *************************************************************************************************
ok: [node-1] => {
    "msg": "Hello world!"
}

TASK [Show the output] **********************************************************************************************
ok: [node-1] => {
    "msg": {
        "changed": false,
        "failed": false,
        "msg": "Hello world!"
    }
}
```

So every task has at least:

- `changed` - (boolean).
- `failed` - (boolean).

For `changed`, typically a `[handler]s`(handlers) is used. More on that topic later.
For `failed`, typically  a `[block]s`(blocks) is used. More on that topic later.

Some tasks have more "Return Values" (documented for each module). For example [`command`](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/command_module.html) has a `stdout`.

```yaml
- name: Run some command
  ansible.builtin.command:
    cmd: "echo 'Hello world!'"
  register: output

- name: Show the output
  debug:
    msg: "{{ output.stdout }}"
```

Will produce:

```text
TASK [Run a command] *************************************************************************************************
changed: [node-1]

TASK [Show the output] ***********************************************************************************************
ok: [node-1] => {
    "msg": {
        "changed": true,
        "cmd": [
            "echo",
            "Hello world!"
        ],
        "delta": "0:00:00.007194",
        "end": "2023-05-10 14:42:17.537168",
        "failed": false,
        "msg": "",
        "rc": 0,
        "start": "2023-05-10 14:42:17.529974",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "Hello world!",
        "stdout_lines": [
            "Hello world!"
        ]
    }
}
```

As this (`output`) is just a variable, you can use it in a later task:

```yaml
- name: Show something
  debug:
    msg: "The output contains more than 1 line."
  when:
    - output.stdout_lines | length > 1

- name: Show each line one by one
  debug:
    msg: "{{ item }}"
  loop: "{{ output.stdout_lines }}"
```

> Note: Especially for people that use shell-scripts, the above pattern may seem attractive, but using `ansible.builtin.command` and `ansible.builtin.shell` is not recommended. Use the modules that are available for the task you want to perform.

## Assignment

Write a playbook that uses the Ansible [find](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/find_module.html) module to find all files in "/tmp". The playbook should print the mode of the file.

The solution can be found [here](https://github.com/robertdebock/learn-ansible-solutions/tree/master/registering_results).

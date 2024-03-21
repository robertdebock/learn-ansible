# Check mode

Check mode is a feature of Ansible that allows you to run a playbook or task without actually making any changes to the system. This is useful to see what changes would be made without actually making them.

There are some scenarios where this can help:

- You want to see what changes would be made before actually making them.
- There is a task that runs on the condition that an earier task has (not) made changes. You can use check mode to see if the earlier task would make changes.

The last scenario can be seen here:

```yaml
- name: Check if nginx is installed
  ansible.builtin.package:
    name: nginx
  check_mode: true
  register: nginx_installed

- name: Upgrade package
  ansible.builtin.package:
    name: nginx
    state: latest 
  when:
    - nginx_installed.results is defined
```

> Note: The above example is quite an edge-case. You will not see this often.

## Running

To run a playbook in `check` mode, use the `--check` parameter:

```bash
ansible-playbook --check playbook.yml
```

> Note: You can also add `--diff` to learn what Ansible would change. Some modules do not support `--diff`, in which case you will not see differences.

## Caveats

When you run a playbook in check mode, Ansible will not actually do anything. Take this example:

```yaml
- name: Install a package
  ansible.builtin.package:
    name: nginx

- name: Start the service
  ansible.builtin.service:
    name: nginx
    state: started
```

The above task-list can fail, because the package `nginx` is not actually installed when running in check mode. The service `nginx` can't be started when the package is not installed.

## Assignment

Take any of the previous assignments and run it in check mode.

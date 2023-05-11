# Handler

Handlers are tasks that are only run when "notified". They are typically used to restart a service when a configuration file has changed.

`tasks/main.yml`:

```yaml
- name: Install Apache
  ansible.builtin.package:
    name: apache2
  notify:
    - Restart Apache
```

`handlers/main.yml`:

```yaml
- name: Restart Apache
  ansible.builtin.service:
    name: apache2
    state: restarted
```

> Note: The `notify` is a list, so you can notify multiple handlers.

If more than one handlers are called, the order of the execution of the handlers is as the order is in the `handlers/main.yml` (or `handlers:` parameter).

For example:

`tasks/main.yml`:

```yaml
- name: Do something
  ansible.builtin.command:
    cmd: ls
  notify:
    - second handler
    - first handler
```

`handlers/main.yml`:

```yaml
- name: first handler
  ansible.builtin.debug:
    msg: "first handler"

- name: second handler
  ansible.builtin.debug:
    msg: "second handler"
```

This would result in this order:

1. Do something
2. first handler
3. second handler

There is no mechanism to clear handlers; once called, they will run at the end of a succesful play. A failed play will not run the handlers.

You can ask Ansible to run the handlers at a specific moment using the `meta` module:

`tasks/main.yml`:

```yaml
- name: Do something
  ansible.builtin.command:
    cmd: ls
  notify:
    - first handler

- name: Run handlers
  ansible.builtin.meta: flush_handlers

- name: Do something else
  ansible.builtin.command:
    cmd: ls
```

`handlers/main.yml`:

```yaml
- name: first handler
  ansible.builtin.debug:
    msg: "first handler"
```

The order will be:

1. Do something
2. Run handlers
3. first handler
4. Do something else

## Incorrect way

An different, incorrect way to restart a service on a change would be:

```yaml
# INCORRECT!

- name: Install Apache
  ansible.builtin.package:
    name: apache2
  register: output

# INCORRECT!
- name: Restart Apache
  ansible.builtin.service:
    name: apache2
    state: restarted
  when:
    - output.changed
```

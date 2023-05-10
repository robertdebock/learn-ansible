# Looping

Sometimes you want to repeat a task for several items. This can be done using a `loop`.

> Note: `loop` is more recent; looping used to be done using `with_items`. Both are fine, if you have are writing code, using `loop` is preferred.

```yaml
- name: Install tools
  hosts: all

  tasks:
    - name: Install tool
      apt:
        name: "{{ item }}"
      loop:
        - tcpdump
        - screen
        - nc
```

The a above task will run 3 times, once for each item in the list.

Sometimes you want to loop over a list of items with extra parameters:

```yaml
- name: Install tools
  hosts: all

  tasks:
    - name: Install tool
      apt:
        name: "{{ item.name }}"
        state: "{{ item.state }}"
      loop:
        - name: tcpdump
          state: present
        - name: screen
          state: absent
```

The above task will run 2 times, once for each item in the list. Each item is a dictionary with 2 keys: `name` and `state`. Using a value from a dictionary is done using the dot notation: `item.name` or `item.state`.

You can make the above code even cooler by using a default value of `present`.

```yaml
- name: Install tools
  hosts: all

  tasks:
    - name: Install tool
      apt:
        name: "{{ item.name }}"
        state: "{{ item.state | default('present') }}"
      loop:
        - name: tcpdump
        - name: screen
          state: absent
```

In the above example, the package under `name` will be installed by default. Only when specifying `state: absent` the package will be removed.

## loop_control

When looping over a list of dictionaries, the output can become cluttered. You can use `loop_control` to change the output.

```yaml
- name: Install tools
  hosts: all

  tasks:
    - name: Install tool
      apt:
        name: "{{ item.name }}"
        state: "{{ item.state | default('present') }}"
      loop:
        - name: tcpdump
        - name: screen
          state: absent
      loop_control:
        label: "{{ item.name }}"
```

Now the ouput will not show the whole dictionary, but only the value of `item.name`.

Instead of `item`, you can also rename the variable to something else:

```yaml
- name: Install tools
  hosts: all

  tasks:
    - name: Install tool
      apt:
        name: "{{ tool.name }}"
        state: "{{ tool.state | default('present') }}"
      loop:
        - name: tcpdump
        - name: screen
          state: absent
      loop_control:
        label: "{{ tool.name }}"
        loop_var: tool
```

This renaming can be useful when looping over a list of dictionaries, with in the dictionary another list. Without renaming, you may end up with something like `{{ item.item.name }}`.

## Assignment

Create a playbook to create users. The playbook should create 2 [user](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html)s, with a specific `name` and `comment`.

> Note: you can add `--check` or `-C` to run the playbook without making any changes. Sort of a dry-run to see what would happen.

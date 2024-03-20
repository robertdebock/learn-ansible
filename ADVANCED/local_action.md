# Local Action

In some situations you can execute a task on a local host, the host where Ansible is running.

Imagine you need to find an IP address of a hostname. This task does not need to be executed on a remote host, as the information is available on the any host, including the Ansible controller, where Ansible is running.

You can either use the `delegate_to` parameter or the `local_action` module. The next example does the exact same thing, so pick the one you like the most.

## Local action

```yaml
- name: Configure /etc/hosts
  hosts: all

  tasks:
    - name: Find IP address of a hostname
      local_action:
        module: ansible.builtin.command
        cmd: "dig +short {{ inventory_hostname }}"
      register: ip_address
    
    - name: Write the IP address to /etc/hosts
      ansible.builtin.lineinfile:
        path: /etc/hosts
        create: yes
        line: "{{ ip_address.stdout_lines[0] }} {{ inventory_hostname }}"
```

## Delegate to

```yaml
- name: Configure /etc/hosts
  hosts: all

  tasks:
    - name: Find IP address of a hostname
      ansible.builtin.command: "dig +short {{ inventory_hostname }}"
      register: ip_address
      delegate_to: localhost
    
    - name: Write the IP address to /etc/hosts
      ansible.builtin.lineinfile:
        path: /etc/hosts
        create: yes
        line: "{{ ip_address.stdout_lines[0] }} {{ inventory_hostname }}"
        regexp: "^{{ ip_address.stdout_lines[0] }} "
```

> NOTE: The command `dig` needs to be available on the host where Ansible is running. You could write a task for it.

Situations where this may be useful:

- You need to (un-)register a host from some kind of tool.
- You want to keep track of the last time a playbook was executed.
- You want to send a message to a chat tool.

## Assignment

Write a small playbook that writes the date as found on a machine, on the host where Ansible is running using either `local_action` or `delegate_to`.

> Note 1: The date is stored in a variable called `ansible_date_time`.
> Note 2: Use `serial: 1` in order to write results machine-by-machine, preventing overwrites.

## Solution

See [this playbook setup](https://github.com/robertdebock/learn-ansible-solutions/blob/master/local_action/).

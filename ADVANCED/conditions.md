# Conditionals

You can tell Ansible to run a taks under certain "conditions".

For example; "Run this task only when the OS is Ubuntu." would result in a playbook as such:

```yaml
- name: Install Apache on Ubuntu
  hosts: webservers

  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
      when:
        - ansible_distribution == "Ubuntu"
```

The `when` statement is a conditional. It can be a single condition, or a list of conditions. The task will only run when all conditions are met. Using `or` is also possbile.

You can also use [filters](filters) to check for conditions.

```yaml
- name: Install Apache on Ubuntu
  hosts: webservers

  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
      when:
        - ansible_distribution == "Ubuntu"
        - ansible_distribution_major_version | int >= 18
```

You can go all-in with conditions. For me combining `and`, `or` and `not` is difficult to understand. Consider this example:

```yaml
- name: Install Apache on Ubuntu
  hosts: webservers

  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
      when:
        - ansible_distribution == "Ubuntu"
        - ansible_distribution_major_version | int >= 18
        - ansible_distribution_major_version | int < 20
        - ansible_distribution_release != "focal" or
          ansible_distribution_release != "bionic"
```

The above list of conditionals is difficult to read and understand.

## Assignment

> Note: This assignment requires the [previous assignment on inventory](inventory) to be completed.

Change this task to run only on Ubuntu 18 and 19.

```yaml
    - name: Print hello world
      ansible.builtin.debug:
        msg: "Hello World!"
```

For this task, the playbook needs to be told to `gather_facts`.

In the `inventory`, you can set `ansible_connection=local` to emulate a remote host. This tells Ansible that the specific node can be reached locally. Normally, this is only done for `localhost`.

> Note: The tasks will only run when the condition is met. (Ubuntu 18 or Ubuntu 19.) The `ansible_connection=local` setting will run the job on your machine. You can change the condition to run on your specific node.

## Verify

Run `ansible-playbook playbook.yml`. You should see this output:

```text
PLAY [Print something] *******************************************************************************************

TASK [Gathering Facts] *******************************************************************************************

TASK [Print hello world] *****************************************************************************************
ok: [node-1] => {
    "msg": "Hello World!"
}
ok: [node-3] => {
    "msg": "Hello World!"
}
```

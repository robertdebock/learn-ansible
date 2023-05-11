# Strategy

The play parameter `strategy` can be used to change Ansibles ordering behaviour. There are a couple of available strategies:

- `linear` - (default) Run tasks in order.
- `free` - Run tasks in parallel. When running on multiple hosts, a next task runs as fast as the host is done with the current task. Use this when order is not important.
- `debug` - Run linearly, but ask to run each job.


Here is an example of a playbook using the `free` strategy:

```yaml
- name: Install Apache HTTPD
  hosts: webservers
  strategy: free

  tasks:
    - name: Install Apache HTTPD
      ansible.builtin.package:
        name: apache2
        state: present
    
    - name: Start Apache HTTPD
      ansible.builtin.service:
        name: apache2
        state: started
```

The above playbook would run the two tasks on all [targeted](targeting) nodes in any random order. For example, the order could be:

| Strategy linear                                      | Strategy free                     |
| ---------------------------------------------------- | --------------------------------- |
| 1. Install Apache HTTPD on node-1, node-2 and node-2 | 1. Install Apache HTTPD on node-1 |
| 2. Start Apache HTTPD on node-1, node-2 and node-3   | 2. Install Apache HTTPD on node-2 |
|                                                      | 3. Start Apache HTTPD on node-2   |
|                                                      | 4. Install Apache HTTPD on node-3 |
|                                                      | 5. Start Apache HTTPD on node-1   |
|                                                      | 6. Start Apache HTTPD on node-3   |

## Assignment

1. Create a playbook that has 2 tasks, one that randomly sleeps up to 30 seconds and another that displays a message. (You can use `command` to run `sleep {{ 30 | random }}`.)
2. Set the strategy to `free`.

Run the playbook and see that hosts start a task whenever their previous task is done.

## Solution

I've made a [playbook here](https://github.com/robertdebock/learn-ansible-solutions/blob/master/strategy/).

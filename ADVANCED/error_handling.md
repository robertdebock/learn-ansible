# Error handling

In some cases, Ansible will run into an error and you'd like to continue.

Imagine you want to test something on a remote host, and in case the test fails, run some task.

```yaml
- name: Show blocks.
  become: yes

  tasks:
    - name: Try writing to /tmp/testing.txt
      block:
        - name: Writing to /tmp/testing.txt
          ansible.builtin.file:
            path: /tmp/testing.txt
            state: touch
      rescue:
        - name: Inform that /tmp/testing.txt is unwritable
          debug:
            msg: "No, I can't write to /tmp/testing.txt"
    
    - name: Just anothor task
      ansible.builtin.debug:
        msg: "This task will just run."
```

Sometimes this mechanism can be used to continue on a "small" error.

## Assignment

Write a task using `ansible.builtin.command` to see if the file `/tmp/bla.txt` can be listed.

> Note: There are better ways to do this, but this is just an example.

Use `block` and `rescue` to write a [line in a file](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/lineinfile_module.html):

- Put "YES: /tmp/bla.txt" in /tmp/results.txt when the file exists.
- Put "NO: /tmp/bla.txt" in /tmp/results.txt when the file does not exist.

# Data structures

There are a couple of variable types in Ansible. The most common ones are:

- string
- integer
- boolean
- list
- map or dictionary

## String

This is just a value like:

```yaml
my_string: "Hello world!"
```

You can also express integers (numbers like `1`, `2`, etc.) and booleans as a string:

```yaml
my_string_that_looks_like_an_integer: "1"
my_string_that_looks_like_a_boolean: "yes"
```

In the above bases, the value is a string, not an integer or boolean.

## Integer

This is a number, like:

```yaml
my_integer: 1
```

Integers can be used to calculate or compare values.

```yaml
- name: Show the value of 1 + 1
  debug:
    msg: "{{ 1 + 1 }}"

- name: See if the value is 1 or up
  debug:
    msg: Yes, {{ number }} is larger than 1."
  when: number > 1
```

## Boolean

A boolean is a value that can be `true` or `false`. There are a couple of ways to express a boolean:

- `true` or `false` (Casing does not matter.)
- `yes` or `no` (Casing does not matter.)
- `on` or `off` (Casing does not matter.)
- `1` or `0`

I find `yes` and `no` easy to read, and find `1` and `0` hard to read. [Ansible Lint](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html) and [yamllint](https://yamllint.readthedocs.io) both advice to use `true` and `false`.

Running a job conditionally is a common use-case for a boolean. The syntax is a bit weird:

```yaml
- name: Run this task only when the variable is true
  debug:
    msg: "Some message."
  when:
    - my_boolean
```

## List

A list is a collection of values. A list is expressed like this:

```yaml
my_list:
  - value1
  - value2
  - value3
```

A list may contain any type of value, including other lists or dictionaries.

```yaml
my_list:
  - name: value1
  - name: value2
    something: foobar
  - name: value3
    another_things:
      - name: foo
      - name: bar  
```

## Dictionary or map

A dictionary or map is a collection of key-value pairs. A dictionary is expressed like this:

```yaml
my_dictionary:
  key1: value1
  key2: value2
  key3: value3
```

A dictionary looks a bit like the contents of a fysical dictionary, a word and a description.

## Assignment

Describe your family in an Ansible list called `family_members`. Each family member should have a name, age and a list of hobbies.

## Verify

Have a look at [this example](https://github.com/robertdebock/learn-ansible-solutions/blob/master/data_structures/variables.yml).

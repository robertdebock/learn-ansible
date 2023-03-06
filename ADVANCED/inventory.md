# Inventory

The inventory is an important place; setting up the inventory, group vars and host vars correctly helps to keep your data [dry](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself), ensures that it's easy to find data and that you are able to continue with the correct structure for years to come.

## Inventory file

The inventory file describes the targets available.

### Text or executable?

Remember that a non-executable file (a text file) is read, wher as an executable file will be executed. The executable version would be called a "dynamic inventory".

### Formats

The inventory file can be written in [INI](https://en.wikipedia.org/wiki/INI_file), [YAML](https://yaml.org) and even [JSON](https://www.json.org/json-en.html).

> I would advice to use the INI-format.

### Locations

By [default](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html) the inventory is placed in `/etc/ansible/hosts`. This would be a system-wide inventory, which is not very common to use.

You can override the inventory by specifying the `-i` parameter:

```shell
ansible-playbook -i /path/to/inventory my_playbook.yml
```

I would recommend to place an `ansible.cfg`, referring to an `inventory` in the project you are working on.

`ansible.cfg`:
```ini
[defaults]
inventory = inventory
```

`inventory/hosts`
```ini
[webservers]
node-a
node-b

[databaseservers]
node-c
node-d

[development]
node-a
node-c

[production]
node-a
node-d

[applicationx:children]
webservers
databaseservers
```

Good to know:

- Hosts can be in multiple groups.
- You can place variable and values in an inventory, I think [`group_vars`](group_host_vars) is a better place.
- You can make cross-selections. This is described in the [targeting](targeting) chapter.
- Groups-in-groups are possible, using `:children`.

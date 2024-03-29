# Inventory

The inventory is an important place; setting up the inventory, group vars and host vars correctly helps to keep your data [dry](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself), ensures that it's easy to find data and that you are able to continue with the correct structure for years to come.

## Inventory file

The inventory file describes the targets available.

### Text or executable?

Remember that a non-executable file (a text file) is simply read, where as an executable file will be executed and the output is used as an inventory. The executable version would be called a "dynamic inventory".

A non-executable file is a text file, that contains the hosts and groups the hosts. We'll be using this type of inventory in the next chapters.

A few examples of dynamic inventory scripts:

- [AWS](https://docs.ansible.com/ansible/latest/collections/amazon/aws/aws_ec2_inventory.html)
- [Azure](https://docs.ansible.com/ansible/latest/collections/azure/azcollection/azure_rm_inventory.html)
- [GCP](https://docs.ansible.com/ansible/latest/collections/google/cloud/gcp_compute_inventory.html)

### Formats

The inventory file can be written in [INI](https://en.wikipedia.org/wiki/INI_file), [YAML](https://yaml.org) and even [JSON](https://www.json.org/json-en.html).

> I would advice to use the INI-format as it's pretty easy to read and write.

### Locations

By [default](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html) the inventory is placed in `/etc/ansible/hosts`. This would be a system-wide inventory, which is not very common to use.

You can override the inventory by specifying the `-i` parameter:

```shell
ansible-playbook -i /path/to/inventory my_playbook.yml
```

I would recommend to place an `ansible.cfg`, referring to an `inventory` in the project or repository you are working on.

Here is an example of an inventory in the INI-format:

### `ansible.cfg`

```ini
[defaults]
inventory = inventory
```

### `inventory/hosts`

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
- You can place variables and values in an inventory, [`host_vars` or `group_vars`](group_host_vars) would be a better place.
- You can make cross-selections. This is described in the [targeting](targeting) chapter.
- Groups-in-groups are possible, using [`:children`](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#grouping-groups-parent-child-group-relationships).
- If `ansible.cfg` refers to an `inventory`, this `inventory` can be a file or directory.
- You can use `-i` to override the inventory. A drawback is that these "runtime" options are not stored as code.

## Assignment

Create a new directory somewhere on your system, and create a new file in that directory called `ansible.cfg` file that refers to an `inventory` file.

Write an inventory (the solution I used will use `ini` format, you can pick a different format) with the following groups:

- `webservers`
- `databaseservers`
- `netherlands`
- `switzerland`

Add the following hosts:

- `node-1` (a Dutch webserver)
- `node-2` (a Swiss webserver)
- `node-3` (a Dutch database server)
- `node-4` (a Swiss database server)

Make a group of groups: `europe` that contains `netherlands` and `switzerland`.

### Verify

Run `ansible all --list-hosts`. You should see this output:

```text
  hosts (4):
    node-1
    node-2
    node-3
    node-4
```

Run `ansible-inventory --list`. You should see this output:

```json
{
    "_meta": {
        "hostvars": {}
    },
    "all": {
        "children": [
            "ungrouped",
            "webservers",
            "databaseservers",
            "europe"
        ]
    },
    "databaseservers": {
        "hosts": [
            "node-3",
            "node-4"
        ]
    },
    "europe": {
        "children": [
            "netherlands",
            "switzerland"
        ]
    },
    "netherlands": {
        "hosts": [
            "node-1",
            "node-3"
        ]
    },
    "switzerland": {
        "hosts": [
            "node-2",
            "node-4"
        ]
    },
    "webservers": {
        "hosts": [
            "node-1",
            "node-2"
        ]
    }
}
```

Run `ansible-inventory --graph`. You should see this output:

```text
@all:
  |--@ungrouped:
  |--@webservers:
  |  |--node-1
  |  |--node-2
  |--@databaseservers:
  |  |--node-3
  |  |--node-4
  |--@europe:
  |  |--@netherlands:
  |  |  |--node-1
  |  |  |--node-3
  |  |--@switzerland:
  |  |  |--node-2
  |  |  |--node-4@all:
  |--@ungrouped:
  |--@webservers:
  |  |--node-1
  |  |--node-2
  |--@databaseservers:
  |  |--node-3
  |  |--node-4
  |--@europe:
  |  |--@netherlands:
  |  |  |--node-1
  |  |  |--node-3
  |  |--@switzerland:
  |  |  |--node-2
  |  |  |--node-4
```

### Solution

The solution can be found in the [solution](https://github.com/robertdebock/learn-ansible-solutions/tree/master/inventory) directory.

> NOTE: This assignment is required for further chapters.

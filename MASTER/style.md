# Style

Here are some style tips that I like to use. Style is a preference so there are different, correct other styles.

## Omit defaults

Rule: Whenever a parameter has a default, I tend to not mention it.
Reason: Less text, more readable, easier to maintain.

```yaml
- name: Install something
  ansible.builtin.package:
    name: some_package
    # state: preset
    # The `state` is `present` by default.
```

## Use generic modules

Rule: Use generic modules (`package`, `service`) over specific modules (`apt`, `yum`, `systemd`, `service`).
Reason: The module `package` is more generic and can handle more package managers.

```yaml
- name: Install something
  ansible.builtin.package:
    name: some_package
```

| Specific module | Generic module |
| --------------- | -------------- |
| `apt`           | `package` |
| `yum`           | `package` |
| `systemd`       | `service` |
| `service`       | `service` |

> Note: The "non-FQCN" modules are mentioned to keep the table readable. The FQCN is `ansible.builtin.package` and `ansible.builtin.service`.

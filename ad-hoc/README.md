
## Ad Hoc Commands

This section covers ad-hoc commands.

## Note on Defaults

Due to my `ansible.cfg` having my inventory as a default:

```
[defaults]
inventory = inventory
```

I am not passing `ansible -i`, if you don't have that in your config, then this:

```
ansible somegroup -m ping
```

Becomes:

```
ansible -i inventoryfile somegroup -m ping
```

### Ping Module

Ping all the targets defined in your inventory file:

```
$ ansible all -m ping
```

Ping the `my.laptop` target defined in your inventory file:

```
$ ansible my.laptop -m ping
```

### Gather Facts

```
$ ansible all -m gather_facts
```

Gather Facts for only one host:

```
$ ansible all -m gather_facts --limit 10.0.0.2
```

### Listing Hosts for a Group

List Hosts:

```
$ ansible all --list-hosts
```

## Commands

To run the uptime command on all hosts:

```
$ ansible all -m command -a hostname
```

When using the shell module when you are using arguments:

```
$ ansible all -m shell -a "uptime --help"
```

Update index repositories with apt

```
$ ansible all -m apt -a update_cache=true --become --ask-become-pass
```

Install packages with apt:

```
$ ansible all -m apt -a name=vim --become --ask-become-pass
```

Install the latest package with apt:

```
$ ansible all -m apt -a "name=vim state=latest" --become --ask-become-pass
```

Upgrade dist with apt:

```
$ ansible all -m apt -a upgrade=dist --become --ask-become-pass
```

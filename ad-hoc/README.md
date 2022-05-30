
## Modules

Ping all the targets defined in your inventory file:

```
$ ansible all -m ping
```

Ping the `my.laptop` target defined in your inventory file:

```
$ ansible my.laptop -m ping
```

Gather Facts:

```
$ ansible all -m gather_facts
```

Gather Facts for only one host:

```
$ ansible all -m gather_facts --limit 10.0.0.2
```

List Hosts:

```
$ ansible all --list-hosts
```

## Ad Hoc Commands

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

# ansible-playground
Collection of Ansible Resources

## About

This is a collection of my ansible resources

## Installation

Installing ansible with virtualenv and pip:

```
python3 -m pip virtualenv -p python3 .venv
source .venv/bin/activate
pip install ansible==4.4.0
```

## Configuration

Your servers will be defined in a inventory file which we will name `inventory`, which can look something like this:

```
[local]
my.laptop  ansible_connection=local

[webservers]
10.0.2.10  ansible_host=10.0.2.10
10.0.2.11  ansible_host=10.0.2.11

[local:vars]
deprecation_warnings = False
```

More info on inventory can be found here:
- https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html

## Verifying Connectivity

Let's start with the `inventory` file:

```
[localhost]
my.laptop       ansible_connection=local
my.workstation  ansible_connection=local
```

And then we can use the `ping` module, to ensure that ansible can reach our `localhost` group or `my.laptop` target.

To target the host only:

```
$ ansible -i inventory my.laptop -m ping
my.laptop | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
```

To target the group (this is useful when theres more than one target in a group:

```
$ ansible -i inventory localhost -m ping
my.workstation | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
my.laptop | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
```

## Running Ad-Hoc Commands

See [ad-hoc](ad-hoc/README.md)

## Working with Local 



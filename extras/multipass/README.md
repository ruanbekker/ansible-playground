# Multipass

Create a SSH Key:

```bash
ssh-keygen -f ~/.ssh/ansible -t rsa -N '' -C "ansible-workshop-key"
```

Copy the contents of the public key:

```
cat ~/.ssh/ansible.pub
```

And replace the `<paste-ssh-public-key-here>` in the `cloudinit.yml` file:

```
#cloud-config
groups:
  - admins

users:
  - default
  - name: ubuntu
    groups: admins
    sudo:  ALL=(ALL) NOPASSWD:ALL
    ssh_authorized_keys:
      - <paste-ssh-public-key-here> 

package_update: true

packages:
 - apt-transport-https
 - gnupg
 - ca-certificates
 - wget
 - curl
 - apt-utils
 - net-tools
 - python3
```

Then create 2 VMs, `web-server-01` and `web-server-02`:

```bash
$ for vm in web-server-01 web-server-02; do multipass launch --name $vm --cpus 1 --mem 512M --disk 8G --cloud-init cloudinit.yml focal; done
Launched: web-server-01
Launched: web-server-02
```

Then view the IPs of your VMs:

```bash
$ multipass list
Name                    State             IPv4             Image
web-server-01           Running           192.168.64.27    Ubuntu 20.04 LTS
web-server-02           Running           192.168.64.28    Ubuntu 20.04 LTS
```

You can then access them by SSH (to test):

```bash
$ ssh -i ~/.ssh/ansible ubuntu@192.168.64.27
$ exit
```

When you exited your VM and back to your terminal, now you can populate your inventory file in `inventory` like this:

```
[webservers]
web-server-01 ansible_host=192.168.64.27
web-server-02 ansible_host=192.168.64.28

[webservers:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=~/.ssh/ansible
```

Then you can run the ping module against your webservers group:

```
$ ansible webservers -m ping
web-server-02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
web-server-01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

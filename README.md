# o3-virt-setup


## Features

- Installs a set of common packages
- Makes sure the `stack` user exists 
- Sets up tripleo-common including the temporary/unmerged tripleo-api patch 
- Sets up the validations API
- Sets up tripleo-ui
- Adds cors settings for keystone and ironic
- Opens API ports for tripleo-common/tripleo-ui/validations in iptables config


## TO DO

- Add Mistral setup
- Add Zaqar setup


## Setup

Depending on your machine, the whole setup takes between 

Install Ansible:

```
    sudo yum -y install ansible
```

Please do the following steps as the `stack` user on your host machine.

Check out this repository and create a hosts file:

```
    git clone https://github.com/flofuchs/o3-virt-setup.git
    cd o3-virt-setup
    cp hosts.sample hosts
```

In hosts add the correct values to the [undercloud] section, similar to this:

```
<instack-ip>  ansible_ssh_user=root    ansible_ssh_private_key_file=/home/stack/.ssh/id_rsa
```

Run the playbook:

```
    ansible-playbook -i hosts site.yaml
```


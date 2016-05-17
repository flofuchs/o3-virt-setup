# o3-virt-setup

This playbook intends to facilitate the installation of a couple of
TripleO components which are currently under development on an
instack/undercloud node.


## Prerequisites

This playbook has been tested on CentOS with CentOS as the instack node
OS.

## Features

- Installs a set of common packages
- Makes sure the `stack` user exists
- Sets up rhci-tripleo-api
- Sets up tripleo-common including the initial mistral integration patch
- Sets up the validations API
- Sets up tripleo-ui
- Adds cors settings for keystone and ironic
- Opens API ports for rhci-tripleo-api/tripleo-ui/validations in iptables config


## TO DO

- Add Zaqar setup
- Start APIs in screen or tmux sessions


## Setup

Make sure you have your virtual environment set up:

http://docs.openstack.org/developer/tripleo-docs/environments/environments.html#virtual-environment

Don't proceed with "Installing the undercloud" as it is included in the
playbook.

Depending on your machine, the whole playbook takes a while to complete.
So don't worry if there is no progress for a couple of minutes.

Install Ansible:

```
sudo yum -y install ansible
```

Please do the following steps as the `stack` user on your **host** machine.

Check out this repository and create a hosts file:

```
git clone https://github.com/flofuchs/o3-virt-setup.git
cd o3-virt-setup
cp hosts.sample hosts
```

In the `hosts` file add the correct values to the [undercloud] section,
similar to this:

```
<instack-ip>  ansible_ssh_user=root    ansible_ssh_private_key_file=/home/stack/.ssh/id_rsa
```

If you want the RHCI API to be installed, add the repo url to the hosts file as well:

```
rhci_repo_url = <rhci_repo_url>
```

Run the playbook:

```
ansible-playbook -i hosts site.yaml
```

After the playbook run has completed, log into you undercloud and start
the services (as the `stack` user). You're going to need three terminal
windows, so it's advisable (but not required) to use tmux or screen.

Run the tripleo API:

```
cd ~/rhci-tripleo-api
tox -e venv -- tripleo-api --config-file tripleo.conf
```

Run the validations API:
```
cd ~/clapper/ansible-tests
source .venv/bin/activate
source ~/stackrc
./validation-api.py
```

Run the UI:
```
cd ~/tripleo-ui
gulp
```

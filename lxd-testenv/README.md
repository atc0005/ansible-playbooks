# lxd-testenv playbooks

*Small suite of playbooks intended to help quickly spin up a test environment
for other playbook work.*

## Prerequisites

The following items need to be completed *before* attempting to use the
playbook.

### Generate SSH Public key

`ssh-keygen -t ed25519`

Press Enter repeatedly until the process completes. The new key should ONLY be
used for accessing containers on the test LXD host system.

### Install packages

`sudo apt-get install lxd lxd-client`

### Initial LXD setup

These playbooks require that you first install the LXD daemon and client
packages. After that, run `sudo lxd init`.

Be sure to use NAT, DHCP and setup a bridge network device (NAT). Containers
created and attached to this bridge will be reachable by the host (and each
other), but will not be directly reachable by external systems. You will need
to setup port forwarding rules *or* setup an external bridge for containers
to be externally reachable. Additionally, you will need to reconfigure LXD
to use the new bridge device.

For most (playbook) testing purposes, the NAT bridge will probably be
sufficient.

**Note:** This held true as of LXD 2.0. I have not tested 2.5+ sufficiently
to know what additional steps are needed to safely setup LXD for local testing.

## Usage

### Create new test environment

If you wish to setup containers and the host as separate steps (perhaps for
troubleshooting purposes):

1. `ansible-playbook -i hosts lxd-setup-containers.yml -K`
1. `ansible-playbook -i hosts lxd-setup-host.yml -K`

Alternatively, you can combine the steps from both playbooks by using the
provided "wrapper" playbook:

1. `ansible-playbook -i hosts lxd-setup.yml -K`

### Teardown test environment

`ansible-playbook -i hosts lxd-remove.yml -K`

## References

- [Generate /etc/hosts with Ansible](https://gist.github.com/rothgar/8793800)
- <https://docs.ansible.com/ansible/latest/modules/lxd_container_module.html>
- <https://stackoverflow.com/questions/30226113/ansible-ssh-prompt-known-hosts-issue>

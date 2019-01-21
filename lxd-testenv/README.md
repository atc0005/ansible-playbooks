# lxd-testenv playbook

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

### Initial setup

The `ansible-lxd.yml` playbook requires that you first install the LXD
daemon and client packages. After that, run `sudo lxd init`.

Be sure to use NAT, DHCP and setup a bridge network device (NAT).

**Note:** This held true as of LXD 2.0. I have not tested 2.5+ sufficiently
to know what additional steps are needed to safely setup LXD for local testing.

## Using the `lxd-*.yml` playbooks

### Create new test environment

1. `ansible-playbook -i hosts lxd-setup-containers.yml -K`
1. `ansible-playbook -i hosts lxd-setup-host.yml -K`

### Teardown test environment

`ansible-playbook -i hosts lxd-remove.yml -K`

## References

- [Generate /etc/hosts with Ansible](https://gist.github.com/rothgar/8793800)
- <https://docs.ansible.com/ansible/latest/modules/lxd_container_module.html>
- <https://stackoverflow.com/questions/30226113/ansible-ssh-prompt-known-hosts-issue>

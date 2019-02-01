# lxd-testenv playbooks

*Small suite of playbooks intended to help quickly spin up a test environment
for other playbook work.*

## Limitations

### Linux Distributions

Tested on Ubuntu 16.04+ only. Future versions of these playbooks and associated
roles may add support for additional distributions.

### Docker storage driver

Some Docker storage options are currently incompatible with LXD containers. If
you use ZFS storage with LXD, Docker (as of 18.09 CE) refuses to work if you
try to use anything other than `vfs`. If you use `dir` LXD storage, then you
have the option of using `vfs` (slower, set by default in v1.0 of the
`atc0005.lxd-testenv` role) or `overlay`. The `overlay` Docker storage driver
is incompatible with ZFS LXD storage, so it is a choice that you will have to
intentionally opt-in to using.

To set this value (or any other Docker storage driver), set the
`lxd_containers_docker_storage_driver` variable within the `lxd-setup.yml`
playbook or another file that you include from the playbook. As of this
writing an override to enable use of the `overlay` driver is commented out
in the appropriate place in the playbook.

## Prerequisites

### Inventory

To effectively use this collection of `lxd-testenv` playbooks you will need to
make sure you configure a test inventory entirely separate from your main
production or development environment. Roles used by this playbook do not
have the required polish and lack sufficient testing to safely interact with
any system of lasting value.

To serve as a starting point, a `testing` inventory has been provided that
already includes relevant settings needed by these playbooks.

### LXD packages

These playbooks require that you first install the LXD daemon and client
packages. After that, `sudo lxd init` should be used. The `atc0005.lxd-host`
role is intended to handle these steps, but as of this writing is only a stub
role. For now, install the `lxd` and `lxd-client` packages and then run `sudo
lxd init` and follow the instructions.

### LXD initial setup

Be sure to use NAT, DHCP and setup a bridge network device (NAT). Containers
created and attached to this bridge will be reachable by the host (and each
other), but will not be directly reachable by external systems. You will need
to setup port forwarding rules *or* setup an external bridge for containers
to be externally reachable. Additionally, you will need to reconfigure LXD
to use the new bridge device.

For most (playbook) testing purposes, the NAT bridge will probably be
sufficient.

## Usage

### Install roles

- While it is technically possible to install each role manually, a
  `requirements.yml` file is provided to help automate this step.
- Because `ansible-galaxy` does not yet natively support updating existing
  roles, the example installation instructions provided below include the
  `--force` parameter to *force* pulling in the latest updates as directed by
  the `requirements.yml` file.

#### Option 1: Within your home directory

- `ansible-galaxy install -r requirements.yml --force`

#### Option 2: Within a `roles` directory in the same location as playbooks

- `ansible-galaxy install -r requirements.yml -p roles --force`

#### Option 3: System-wide for all users

- `sudo ansible-galaxy install -r requirements.yml --force`

### Create new test environment

The following steps will prep the host to run LXD containers and then proceed
to spin up several CentOS and Ubuntu containers. By default at least one
additional contain will be spun up as a Docker Host for testing Docker related
playbooks. Edit `site.yml` and disable the inclusion of the `docker-setup.yml`
playbook to disable that step:

- `ansible-playbook -i inventories/testing site.yml -K`

### Teardown test environment

- `ansible-playbook -i inventories/testing lxd-remove.yml -K`

## References

### External

- [Generate /etc/hosts with Ansible](https://gist.github.com/rothgar/8793800)
- <https://docs.ansible.com/ansible/latest/modules/lxd_container_module.html>
- <https://stackoverflow.com/questions/30226113/ansible-ssh-prompt-known-hosts-issue>

### Related repos

- <https://github.com/atc0005/ansible-playbooks>
- <https://github.com/atc0005/ansible-role-lxd-host>
- <https://github.com/atc0005/ansible-role-lxd-testenv>
- <https://github.com/atc0005/ansible-role-docker-host>

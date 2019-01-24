# Ansible Playbooks and Roles incubator project

*or just `ansible-playbooks` for short*

This is an incubator repository for playbooks and roles that I'm actively
developing and prototyping. The plan is to spin off roles into separate
repositories as they get further along in development. For playbooks, I'm not
yet sure where they will go. For now, I plan to group them based on purpose
along with sample content (inventories, documentation, configuration, etc) to
provide the proper context for their use.

Please know that I am a newbie, so there are likely questionable design and
implementation decisions scattered about. Please open an issue for anything
you find out of place.

## Playbooks

### `lxd-testenv`

Small suite of playbooks intended to help quickly spin up a test environment
for other playbook work.

See the [README](lxd-testenv/README.md) for those playbooks for more info.

## Roles

These are some of the roles that are used by playbooks in this repo. Some of
them started off here in this repo before being moved to a dedicated repo for
further development.

- <https://github.com/atc0005/ansible-role-lxd-host>
- <https://github.com/atc0005/ansible-role-lxd-testenv>
- <https://github.com/atc0005/ansible-role-docker-host>

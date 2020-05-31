# ansible-fail2ban

Setup role to install and configure fail2ban.

## Requirements

### For development and testing

* python 3
* yamllint
* ansible
* ansible-lint (pip package)
* molecule (pip package)
* molecule-vagrant (pip package)
* Vagrant
* VirtualBox

### On server

* Ubuntu
  * 16.04
  * 18.04
  * 20.04

## Role Variables

| Variable name  | Type  | Mandatory?  | Default value             | Description |
|----------------|-------|-------------|---------------------------|-------------|
| fail2ban_jail_local     | text  | no          |                           | Place the content of your `jail.local` here |
| fail2ban_jail_d         | array of `text_file` | no | []                  | Your local jail.d configuration files       |
| fail2ban_action_d       | array of `text_file` | no | []                  | Your local action.d configuration files     |
| fail2ban_filter_d       | array of `text_file` | no | []                  | Your local filter.d configuration files     |

### Definition `text_file`

| Variable name  | Type  | Mandatory?  | Default value | Description |
|----------------|-------|-------------|---------------|-------------|
| name           | filename | yes      |               | name of the file |
| content        | text     | yes      |               | content of the text file |

## Dependencies

None so far.

## Usage

### Add to `requirements.yml`

```yaml
- name: setup-fail2ban
  src: https://github.com/borisskert/ansible-fail2ban.git
  scm: git
```

### Minimal `playbook.yml`

```yaml
- name: Minimal
  hosts: all

  roles:
    - role: setup-fail2ban
```

### Typical `playbook.yml`

```yaml
---
- name: Typical
  hosts: all
  become: true

  roles:
    - role: setup-fail2ban
      fail2ban_jail_local: |
        [DEFAULT]
        ignoreip = 127.0.0.0/8 10.0.0.0/8 172.16.0.0/12 192.168.0.0/16
        bantime = 86400
        banaction = route
      fail2ban_action_d:
        - name: route.local
          content: |
            [Definition]
            actionban   = ip route add <blocktype> <ip>
            actionunban = ip route del <blocktype> <ip>

            [Init]
            blocktype = unreachable
      fail2ban_jail_d:
        - name: ssh.local
          content: |
            [sshd]
            enabled = true
            filter = sshd
      fail2ban_filter_d:
        - name: sshd.local
          content: |
            [INCLUDES]
            before = common.conf
```

## Linting

`molecule` is checking the code style of this project, but you can
 check the concrete errors with the following commands: 

### Yamllint

```shell script
yamllint . --strict
```

### Ansible-lint

```shell script
ansible-lint
```

## Testing

Requirements:

* [Vagrant](https://www.vagrantup.com/)
* [VirtualBox](https://www.virtualbox.org/)
* [Ansible](https://docs.ansible.com/)
* [Molecule](https://molecule.readthedocs.io/en/latest/index.html)
* [yamllint](https://yamllint.readthedocs.io/en/stable/#)
* [ansible-lint](https://docs.ansible.com/ansible-lint/)
* [Docker](https://docs.docker.com/)

### Run within docker

```shell script
molecule test
```

### Run within Vagrant

```shell script
 molecule test --scenario-name vagrant --parallel
```

I recommend to use [pyenv](https://github.com/pyenv/pyenv) for local testing.
Within the Github Actions pipeline I use [my own molecule Docker image](https://github.com/borisskert/docker-molecule).

## License

MIT

## Author Information

* [borisskert](https://github.com/borisskert)

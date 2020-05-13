# ansible-fail2ban

Setup role to install and configure fail2ban.

## Requirements

### For development and testing

* python 3
* yamllint (brew package)
* ansible (brew package)
* ansible-lint (brew package)
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
| jail_local     | text  | no          |                           | Place the content of your `jail.local` here |
| jail_d         | array of `text_file` | no | []                  | Your local jail.d configuration files       |
| action_d       | array of `text_file` | no | []                  | Your local action.d configuration files     |
| filter_d       | array of `text_file` | no | []                  | Your local filter.d configuration files     |

### Definition `text_file`

| Variable name  | Type  | Mandatory?  | Default value | Description |
|----------------|-------|-------------|---------------|-------------|
| name           | filename | yes      |               | name of the file |
| content        | text     | yes      |               | content of the text file |

## Dependencies

None so far.

## Usage

### Add to `requirements.yml`

TBD.

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
      jail_local: |
        [DEFAULT]
        ignoreip = 127.0.0.0/8 10.0.0.0/8 172.16.0.0/12 192.168.0.0/16
        bantime = 86400
        banaction = route
      action_d:
        - name: route.local
          content: |
            [Definition]
            actionban   = ip route add <blocktype> <ip>
            actionunban = ip route del <blocktype> <ip>

            [Init]
            blocktype = unreachable
      jail_d:
        - name: ssh.local
          content: |
            [sshd]
            enabled = true
```

## License

MIT

## Author Information

* [borisskert](https://github.com/borisskert)

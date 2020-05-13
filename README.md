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
| jail           | array of `jail_config` | no | []                | Your local jail configuration |

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
- name: Typical
  hosts: all

  roles:
    - role: setup-fail2ban
      jail:
        - name: ssh.local
          content: |
            [sshd]
            enabled = true

            [DEFAULT]
            ignoreip = 127.0.0.0/8 10.0.0.0/8 172.16.0.0/12 192.168.0.0/16
```

## License

MIT

## Author Information

* [borisskert](https://github.com/borisskert)

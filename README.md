# ansible_embyserver

Installs [`emby server`](https://emby.media/) as docker container managed by systemd.

## System requirements

* Docker
* docker-compose
* Systemd

## Role requirements

* (none, so far)

## Tasks

* Create config volume directory for docker containers
* Setup systemd unit file
* Start/Restart systemd service

## Role parameters

| Variable      | Type | Mandatory? | Default | Description           |
|---------------|------|------------|---------|-----------------------|
| embyserver_version | text | no | `'latest'` | The docker image version |
| embyserver_service_name | text | no | `'embyserver'` | The service name |
| embyserver_config_volume_directory | absolute path | no | `/srv/embyserver/config` | The path of the config volume directory |
| embyserver_user_id | user id | no | `666` | The User ID emby is running with |
| embyserver_group_id | group id | no | `666` | The Group ID emby is running with |
| embyserver_interface | network interface | no | `0.0.0.0` | Bound network interface for the web-interface |
| embyserver_http_port | port number | no | `8096` | Network port for the HTTP interface |
| embyserver_https_port | port number | no | `8920` | Network port for the HTTPS interface |
| embyserver_shares | array of `embyserver_share` objects | no | `[]` | The defined share directories |
| embyserver_direct_rendering_enabled | boolean | no | `false` | Specifies if direct rendering is enabled |

## Example Playbook

### Add to `requirements.yml`:

```yaml
- name: install-embyserver
  src: https://github.com/borisskert/ansible_embyserver.git
  scm: git
```

### Example `playbook.yml`:

```yaml
- hosts: servers
  roles:
    - role: install-embyserver
      embyserver_version: 4.5.2.0
      embyserver_service_name: embyserver
      embyserver_config_volume_directory: /srv/embyserver/config
      embyserver_interface: 0.0.0.0
      embyserver_http_port: 8096
      embyserver_https_port: 8920
      embyserver_user_id: 666
      embyserver_group_id: 666
      embyserver_shares:
        - name: movies
          path: /srv/embyserver/movies
        - name: tv
          path: /srv/embyserver/tv
      embyserver_direct_rendering_enabled: true
```

## Testing

Requirements:

* [Vagrant](https://www.vagrantup.com/)
* [Qemu](https://www.qemu.org/libvirt) and [libvirt](https://libvirt.org/)
* [Ansible](https://docs.ansible.com/)
* [Molecule](https://molecule.readthedocs.io/en/latest/index.html)
* [yamllint](https://yamllint.readthedocs.io/en/stable/#)
* [ansible-lint](https://docs.ansible.com/ansible-lint/)
* [Docker](https://docs.docker.com/)

### Run within docker

```shell script
molecule test --scenario-name docker-default && molecule test --scenario-name docker-all-parameters
```

### Run within Vagrant

```shell script
molecule test --scenario-name vagrant-default && molecule test --scenario-name vagrant-all-parameters
```

I recommend to use [pyenv](https://github.com/pyenv/pyenv) for local testing.
Within the Github Actions pipeline I use [my own molecule Docker image](https://github.com/borisskert/docker-molecule).

## License

MIT

## Author Information

* [borisskert](https://github.com/borisskert)

## Further links

* [embyserver @ Docker hub](https://hub.docker.com/r/emby/embyserver)

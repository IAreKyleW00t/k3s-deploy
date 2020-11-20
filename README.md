# k3s-deploy

An Ansible playbook for deploying [k3s](https://k3s.io/) server and worker nodes with any
additional software/configuration that is necessary.

## Usage

You'll first need to install the `community.kubernetes` ansible package:

    ansible-galaxy collection install community.kubernetes

Assuming the [configuration](#configuration) listed below, run the following Ansible commands to configure systems:

    ansible-playbook -i inventories/hosts server.yaml
    ansible-playbook -i inventories/hosts worker.yaml

## Configuration

### File Structure

```
/inventories
  /group_vars
    all.yaml
    server.yaml
    worker.yaml
  /host_vars
    ...
  hosts
/roles
...
raspi.yaml
server.yaml
worker.yaml
```

### Server

```yaml
# inventories/group_vars/server.yaml
k3s_mode: server

certmanager_version: v1.0.4 # or newer
certmanager_email: admin@example.com

rancher_version: v2.5.2 # or newer
rancher_hostname: rancher.example.com
rancher_letsencrypt_email: "{{ certmanager_email }}" # same as cert-manager
```

### Worker

```yaml
# inventories/group_vars/worker.yaml
k3s_mode: worker
k3s_server_url: 'https://<rancher_url>:6443'
k3s_server_token: 'OUTPUT_FROM_SERVER_PLAYBOOK'
```

### Raspi

```yaml
raspi_memory_split: 16
raspi_wifi_country: us
raspi_keyboard_layout: us
raspi_timezone: America/New_York
raspi_locale: en_US.UTF-8

# See: https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-encrypted-passwords-for-the-user-module
raspi_user_password: '$6$rounds=100000$r0MIyXUGOp3lC0Qn$HqKRFHO1Epgx0KUdVzDI9dxOSvKAraFPClIBHBI/OnmC1wmvbfYwVRDIzr2t5zq5dHX3jv2sTzCKx2lRaBy1A0'
```

## Contributing

Contributions are welcome in the form of GitHub [Issues](https://github.com/IAreKyleW00t/k3s-deploy/issues) and [Pull Requests](https://github.com/IAreKyleW00t/k3s-deploy/pulls).

## License

This Ansible playbook is open source and released under the terms of the
[MIT License](https://choosealicense.com/licenses/mit/).

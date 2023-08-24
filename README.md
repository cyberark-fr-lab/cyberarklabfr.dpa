# Ansible Collection - cyberarkfrlab.dpa

A collection of roles for Cyberark Dynamic Privilege Access:  

| Role                          | Description                                      |
|-------------------------------|--------------------------------------------------|
| cyberarkfrlab.dpa.install_ca  | Install DPA certificate authority on target host |
| cyberarkfrlab.dpa.create_user | Create local user on target host                 |

## Using this collection

### Installing the Collection from Ansible Galaxy

Before using this collection, you need to install it with the Ansible Galaxy command-line tool:
```bash
ansible-galaxy collection install cyberarkfrlab.dpa
```

You can also include it in a `requirements.yml` file and install it with `ansible-galaxy collection install -r requirements.yml`, using the format:
```yaml
---
collections:
- name: cyberarkfrlab.dpa
```

Note that if you install the collection from Ansible Galaxy, it will not be upgraded automatically when you upgrade the `ansible` package. To upgrade the collection to the latest available version, run the following command:
```bash
ansible-galaxy collection install cyberarkfrlab.dpa --upgrade
```

You can also install a specific version of the collection, for example, if you need to downgrade when something is broken in the latest version (please report an issue in this repository). Use the following syntax to install version `0.1.0`:

```bash
ansible-galaxy collection install cyberarkfrlab.dpa:==0.1.0
```

See [Ansible Using collections](https://docs.ansible.com/ansible/devel/user_guide/collections_using.html) for more details.

## Example Playbooks

### Example 1 - Install DPA certificate authority and create local users
```yaml
- hosts: all
  tasks:
    - name: "Install DPA certificate authority"
      ansible.builtin.include_role:
        name: cyberarkfrlab.dpa.install_ca
      vars:
        dpa_ca: "ssh-rsa XXXX....XXXX"

    - name: "Create user privileged-dpa-user"
      ansible.builtin.include_role:
        name: cyberarkfrlab.dpa.create_user
      vars:
        dpa_username: "privileged-dpa-user"
        dpa_user_sudo: true

    - name: "Create user standard-dpa-user"
      ansible.builtin.include_role:
        name: cyberarkfrlab.dpa.create_user
      vars:
        dpa_username: "standard-dpa-user"
        dpa_user_sudo: false
```

### Example 2 - Download and instal DPA certificate authority and create local users
```yaml
- hosts: all
  tasks:
    - name: "Download and install DPA certificate authority"
      ansible.builtin.include_role:
        name: cyberarkfrlab.dpa.install_ca
      vars:
        idp_domain: 'company.id.cyberark.cloud'
        isp_subdomain: 'mycompany'
        isp_user: 'dpa-ansible-automation@cyberark.cloud.1234'
        isp_pass: 'A strong password'
        dpa_workspace_type: 'AWS'
        dpa_workspace_id: '123456789123'

    - name: "Create user privileged-dpa-user"
      ansible.builtin.include_role:
        name: cyberarkfrlab.dpa.create_user
      vars:
        dpa_username: "privileged-dpa-user"
        dpa_user_sudo: true

    - name: "Create user standard-dpa-user"
      ansible.builtin.include_role:
        name: cyberarkfrlab.dpa.create_user
      vars:
        dpa_username: "standard-dpa-user"
        dpa_user_sudo: false
```
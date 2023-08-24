# cyberarklabfr.dpa.install_ca

Install Dynamic Privilege Access SSH certificate authority (SSH CA) on a Linux server.

## Features

- [x] Download SSH CA from Cyberark DPA Identity Security Platform (ISP)
- [x] Install SSH CA on the target host

## Role variables

| Variable                | Required      | Default | Choices                                                     | Comments                                                                           |
|-------------------------|---------------|---------|-------------------------------------------------------------|------------------------------------------------------------------------------------|
| dpa_ca                  | no            | N/A     | `ssh-rsa AAAA....AAAA`                                      | Certificate authority                                                              |
| idp_domain              | not `dpa_ca`  | N/A     | `company.id.cyberark.cloud` or `abc1234.my.idaptive.app`    | Identity tenant domain: mycompany.id.cyberark.cloud or abc1234.my.idaptive.app     |
| isp_user                | not `dpa_ca`  | N/A     |                                                             | Service user, must be member of role **DpaAdmin**                                  |
| isp_pass                | not `dpa_ca`  | N/A     |                                                             | Service user password                                                              |
| dpa_workspace_type      | not `dpa_ca`  | N/A     | `AWS`, `Azure`, *`GCP`                                      | Supported cloud providers                                                          |
| dpa_workspace_id        | not `dpa_ca`  | N/A     |                                                             | Id of the AWS organization, Azure subscription or GCP organization                 |

*To be tested

To create the DPA service user properly:
 1. Follow _Step 1_ of the [official documentation](https://docs.cyberark.com/DPA/Latest/en/Content/ISPSS/ISPSS-API-Authentication.htm#Overview)
 2. Add the service user to the members of DpaAdmin role.

## Example Playbook
```yaml
- hosts: all
  tasks:
  - name: "Run role dpa.install_ca - Scenario: dpa_ca is provided"
      ansible.builtin.include_role:
        name: cyberarklabfr.dpa.install_ca
      vars:
        dpa_ca: "ssh-rsa AAAA....AAAA"

    - name: "Run role dpa.install_ca - Scenario: dpa_ca is not provided"
      ansible.builtin.include_role:
        name: cyberarklabfr.dpa.install_ca
      vars:
        idp_domain: 'company.id.cyberark.cloud'
        isp_subdomain: 'mycompany'
        isp_user: 'dpa-ansible-automation@cyberark.cloud.1234'
        isp_pass: 'A strong password'
        dpa_workspace_type: 'AWS'
        dpa_workspace_id: '123456789123'
```

## Run tests

### Prerequisites
Official molecule [installation instructions](https://ansible.readthedocs.io/projects/molecule/installation/)

### Configure and run

A Cyberark ISP environnement and DPA service account are required to test this role. \
Create an `.env.yml` file as such:
```yaml
TEST_IDP_DOMAIN: 'company.id.cyberark.cloud'
TEST_ISP_SUBDOMAIN: 'mycompany'
TEST_DPA_USER: 'dpa-ansible-automation@cyberark.cloud.1234'
TEST_DPA_PASS: 'A strong password'
TEST_DPA_WK_TYPE: 'AWS'
TEST_DPA_WK_ID: '123456789123'
```

Then, run the test command:
```bash
$ molecule test -s install_ca
$ echo $? # Should return 0
```

**NB:** Alternatively, TEST_* variables can be defined as environnement variables.

## License

GPLv3

## Author Information

Jérôme Coste   
contact@jeromecoste.fr | jerome.coste@cyberark.com
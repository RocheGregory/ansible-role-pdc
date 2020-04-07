# ansible-pdc

![Travis CI](https://img.shields.io/travis/justin-p/ansible-pdc?style=flat-square)

This role will create a brand new Primary Domain Controller with a Active Directory Domain/Forest. No hardening is applied.

Based of the work done by [@jborean93](https://github.com/jborean93) in [jborean93/ansible-windows](https://github.com/jborean93/ansible-windows)

Works on

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012R2

## Requirements

- `python3-winrm` (`pywinrm`) is needed for WinRM.

## Role Variables

All variables listed in `default/main.yml` reference variables from `vars/main.yml`.  
If you want to change any variables overwrite the ones listed in `vars/main.yml`.

### `vars/main.yml`

| Variable                         | Default value                       | Explanation |
|:---------------------------------|:------------------------------------|:------------|
| pdc_administrator_username       | Administrator                       | Settings this to Built-in Administrator account ensure that we know the password of NETBIOS\Administrator. 9/10 times you should leave this to the default value. |
| pdc_administrator_password       | P@ssw0rd!                           | The password of Built-in Administrator account. This password (if pdc_administrator_username left to the default value) will become the password of NETBIOS\Administrator. Change this to a strong password. |
| pdc_domain                       | ad.example.test                     | The Domain of the new Active Directory Forest. For testing\lab purposes it's recommend to use [ad.domain.test](https://www.wikiwand.com/en/.test). For production it's recommend to use a existing domain with a ad subdomain: `ad.domain.tld` |
| pdc_netbios                      | TEST                                | The NetBIOS of the new Active Directory Forest. Change this to something logical that is not AD. |
| pdc_domain_path                  | dc=ad,dc=example,dc=test            | The Distinguished Name of the domain. This should match the value given in pdc_domain (Example: `dc=ad,dc=domain,dc=test`) |
| pdc_domain_safe_mode_password    | P@ssw0rd!                           | The Domain Safe Mode password. Change this to a strong password. |
| pdc_domain_functional_level      | Default ([Windows2008R2](https://github.com/MicrosoftDocs/windows-powershell-docs/blob/master/docset/windows/addsdeployment/Install-ADDSForest.md#-domainmode)) | Specifies the domain functional level of the first domain in the creation of a new forest. The domain functional level cannot be lower than the forest functional level, but it can be higher. Change this depending on your needs. |
| pdc_forest_functional_level      | Default ([Windows2008R2](https://github.com/MicrosoftDocs/windows-powershell-docs/blob/master/docset/windows/addsdeployment/Install-ADDSForest.md#-forestmode)) | Specifies the forest functional level for the new forest. The default forest functional level in Windows Server is typically the same as the version you are running. Change this depending on your needs. |
| pdc_delayed_services             | ["WinRM"]                           | Services that should have the start type set to delayed. Defaults to WinRM to ensure Ansible can connect after a reboot. 9/10 times you should leave this to the default value.  |
| pdc_required_psmodules           | ["ActiveDirectoryDsc"]              | PowerShell/DSC modules to install from the PSGallery. Defaults to [ActiveDirectoryDsc](https://github.com/dsccommunity/ActiveDirectoryDsc) for the `WaitForAD`-check. 9/10 times you should leave this to the default value, always make sure to include `ActiveDirectoryDsc`. |
| pdc_required_features            | ["AD-domain-services", "DNS"]       | Windows Features that should be installed on the Domain Controller. Defaults to AD-domain-services and DNS. 9/10 times you should leave this to the default value. |

## Dependencies

WinRM on the windows host should configured for Ansible.

## Example Playbook

    - hosts: primarydomaincontroller
      roles:
         - { role: justin_p.pdc }

## Local Development

This role includes a Vagrantfile that will spin up a local Windows Server 2019 VM in Virtualbox.  
After creating the VM it will automatically run our role.

### Development requirements

`pip3 install pywinrm`

#### Usage

- Run `vagrant up` to create a VM and run our playbook
- Run `vagrant provision` to reapply our playbook
- Run `vagrant destroy -f && vagrant up` to recreate the VM and run our playbook.
- Run `vagrant destroy` to remove the VM.

## License

MIT

## Authors

- Justin Perdok ([@justin-p](https://github.com/justin-p/)), Orange Cyberdefense

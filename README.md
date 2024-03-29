# SSSD

1. [Overview](#overview)
2. [Role Variables](#role-variables)
     * [OS Specific Variables](#os-specific-variables)
3. [Example Playbook](#example-playbook)
4. [Configuration](#configuration)
5. [Development](#development)
5. [License](#license)
6. [Author Information](#author-information)

## Overview

Manage LDAP authentication with **SSSD** (System Security Services Daemon).

Highly inspired by [Lae's system_ldap role][lae sssd galaxy] with minors updates (test only on Debian 9 and maybe on OpenSuse).

## Role Variables

* **sssd__deploy_state**: The desired state this role should achieve [default : `present`].
* **sssd_pkg_state** : State of new sssd packages [default : `latest`].
* **sssd__unwanted_packages_state** : State of unwanted packages that might interfer with SSSD [default : `absent`].
* **sssd_conf_manage** : If SSSD configuration should be managed with this role [default : `true`].
* **sssd_main_conf_path** : Path to set main SSSD's configuration [default : `/etc/sssd/sssd.conf`].
* **sssd_main_conf_tpl** : Template used to generate the previous config file [default : `etc/sssd/sssd.conf.j2`].
* **sssd_mkhomedir** : If home directories should be created at login [default : `true`].
* **sssd_home_path** : Path where home directories are stored [default : `/home`].
* **sssd_shell** : Path to the default shell to use [default : `/bin/bash`].
* **sssd_shell_override** : If shell should be override with the previous value [default : `False`].
* **sssd_sudoers_ldap** : If sudo must look to `sss` the list of sudoers [default : `false`].
* **sssd_nsswitch_manage** : If nsswitch should be managed by the role [default : `false`].
* **sssd_service_name** : SSSD's service name [default : `sssd`].
* **sssd_flush_handlers** : If handlers need to be applied at the end of the role [default : `False`].

### OS Specific Variables

Please see default value by Operating System file in [vars][vars directory] directory.

* **sssd_pkg_list** : The list of packages to install to provide `sssd`.
  * Be careful, `sssd` may need additional packages to be able to establish a TLS connection to a LDAP/AD/… server (such as `ca-certificates`,…).
* **sssd__unwanted_packages_list** : The list of packages to remove.

## Example Playbook

* Use defaults vars :

``` yml
- hosts: serverXYZ
  roles:
    - role: ipr-cnrs.sssd
```

* With a `group_vars/serverxyz.yml` file :

``` yml
sssd_domain: 'dotld'
sssd_uris:
  - ldap://ldap.domain.tld
sssd_search_base: 'ou=People,dc=domain,dc=tld
sssd_bind_dn: 'cn=sssd_user,ou=apps,dc=domain,dc=tld'
```

  * Then you also need to enter the `bind_dn_password` on the remote host (`/etc/sssd/conf.d/sssd_domain.conf`|`/etc/sssd/conf.d/dotld.conf`). If you want to define `bind_dn_password` in a playbook, please be sure to use [Vault][ansible vault] (or any other tool) to cipher your data !

* If you have some other role that need a working sssd configuration, you may want to apply the new configuration :

``` yml
sssd_flush_handlers: True
```

## Configuration

This role will :
* Install needed packages to provide `sssd`.
* Remove packages that might interfer with `sssd` for authentication.
* Manage the default `sssd` configuration file (`/etc/sssd/sssd.conf`).
* Create an additional configuration file to only store the bind_password (`/etc/sssd/conf.d/domain.bind.conf`).
* Remove `sss` directive for `sudoers` in `/etc/nsswitch.conf` file if `sssd_nsswitch_manage` is set.
* Manage `sssd` service.
* Restart `systemd-logind` service.

## Development

This source code comes from our [Gogs instance][sssd source] and the [Github repo][sssd github] exist just to be able to send the role to Ansible Galaxy…

But feel free to send issue/PR here :)

Thanks to this [hook][gogs to github hook], Github automatically got updates from our [Gogs instance][sssd source] :)

## License

[WTFPL][wtfpl website]

## Author Information

Jérémy Gardais
* Source : [on IPR's Gogs][sssd source]
* [IPR][ipr website] (Institut de Physique de Rennes)

[vars directory]: ./vars
[ansible vault]: http://docs.ansible.com/ansible/latest/vault.html
[gogs to github hook]: https://stackoverflow.com/a/21998477
[sssd source]: https://git.ipr.univ-rennes.fr/cellinfo/ansible.sssd
[sssd github]: https://github.com/ipr-cnrs/sssd
[wtfpl website]: http://www.wtfpl.net/about/
[ipr website]: https://ipr.univ-rennes1.fr/
[lae sssd galaxy]: https://galaxy.ansible.com/lae/system_ldap/

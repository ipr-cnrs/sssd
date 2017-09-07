
## v1.1.x

### Fix
* For Debian Stretch ensure to also install `ca-certificates` (fix #2).
* Ensure to add only one time the sudoers line in `/etc/nsswitch.conf` file (fix #3).

## v1.1.1

### Fix
* Remove `sss` directive for `sudoers` in `/etc/nsswitch.conf` file (#1).

## v1.1

### Fix
* Remove additionnal slash for default `sssd_home_path` value.
* Update documentation.

## v1.0

### Features
* Install sssd package.
* Add configuration file for SSSD.
* You need to set bind_password by your own.
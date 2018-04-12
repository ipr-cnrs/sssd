
## v1.x

### Enhancement
* Remove unwanted packages.

## v1.2.1

### Enhancement
* Allow to override shell attribute.

## v1.2

### Enhancement
* nsswitch.conf is modified only is `sssd_nsswitch_manage` is set (fix #5).

### Fix
* Add `libpam-sss` and `libnss-sss` libraries for Debian (fix #6).

## v1.1.4

### Enhancement
* Add the possibility to flush the handlers to apply the new configuration.

## v1.1.3

### Enhancement
* `sssd_bind_password` is now used and can be directly set on a remote host.

## v1.1.2

### Fix
* For Debian Stretch ensure to also install `ca-certificates` (fix #2).
* Ensure to add only one time the sudoers line in `/etc/nsswitch.conf` file (fix #3).
* Ensure to restart `systemd-logind` to avoid 'Failed to create session' error (fix #4).

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

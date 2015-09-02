# login.farm Documentation

Contributions are welcome, please contact me on gpaterno@gpaterno.com

Useful links:

* [NSS and PAM modules](https://github.com/garlsecurity/nss_securepass)
* [Command line tools](https://github.com/garlsecurity/securepass-tools)
* [Wordpress Plugin](https://github.com/garlsecurity/wp-securepass)
* [Drupal Plugin](https://github.com/garlsecurity/securepass-drupal)

# PAM/NSS Module

Debian "jessie" and Ubuntu from 15.04 have the module available, you can install libnss-securepass.
**Note**: Latest Ubuntu LTS 14.04 has a request for backport on:
https://bugs.launchpad.net/trusty-backports/+bug/1403181


If you use CentOS/RHEL/Scientific Linux, you can find the RPMS for version 6 and 7 here:

http://repo.garl.ch/pub/

**Note**: a RedHat bugzilla request have been submitted to include the RPM in EPEL and Fedora. 
If you can, please sponsor it: https://bugzilla.redhat.com/show_bug.cgi?id=1162234


## Example configuration file 

The file is /etc/securepass.conf:

```
[default]
app_id = << YOUR APPLICATION ID >>
app_secret = << YOUR APPLICATION SECRET >>
endpoint = https://beta.secure-pass.net/

[nss]
realm = login.farm
default_gid = 100
default_home = "/home"
default_shell = "/bin/bash"
```

## SSH Configuration

The securepass tools have an openssh filter that can be used to automatically retrive
users' ssh keys from login.farm. First of all, install your securepass tools via RPM/deb
or **pip install securepass**.

Then configure your sshd_config by adding the following line:
```
AuthorizedKeysCommand /usr/bin/sp-sshkey
```

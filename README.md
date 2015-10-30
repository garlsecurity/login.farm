# login.farm Documentation

Having several linux instances and applications in different clouds?

Say you have virtual machines on Amazon, Rackspace or local premises. 
Or perhaps many web apps here and there, but you don't really have an openldap or active directory.

The ***login.farm*** project is here to help centralizing and managing your identities.
It comes also with a built-in multifactor authentication.

If you're willing to help, contributions are welcome: check the [TODO list](TODO.md).
Please contact me on gpaterno@gpaterno.com for coordination.


Useful links:

* [NSS and PAM modules](https://github.com/garlsecurity/nss_securepass)
* [Command line tools](https://github.com/garlsecurity/securepass-tools)
* [Wordpress Plugin](https://github.com/garlsecurity/wp-securepass)
* [Drupal Plugin](https://github.com/garlsecurity/securepass-drupal)

# Install on Linux

Installing on Linux is easy, if you use popular distributions, you will find pre-made packages.

## Debian/Ubuntu
Debian "jessie" and Ubuntu from 15.04 have the module available, you can install libnss-securepass.

**Note**: Latest Ubuntu LTS 14.04 has a request for backport on:
https://bugs.launchpad.net/trusty-backports/+bug/1403181

## RedHat/Fedora/CentOS
If you use CentOS/RHEL/Scientific Linux, you can find the RPMS for version 6 and 7 here:
http://repo.garl.ch/pub/

```
rpm -ihv http://repo.garl.ch/pub/7/x86_64/garl-release-1-0.noarch.rpm
```

**Note**: a RedHat bugzilla request have been submitted to include the RPM in EPEL and Fedora. 
If you can, please sponsor it: 

* https://bugzilla.redhat.com/show_bug.cgi?id=1162234
* https://bugzilla.redhat.com/show_bug.cgi?id=1259061

## openSUSE/SLES
If you use openSUSE/SLES, you can find the repositories on OBS.

SLES 12: 
```
zypper ar http://download.opensuse.org/repositories/home:/gpaterno:/SecurePass/SLE_12/home:gpaterno:SecurePass.repo
```
openSUSE 13.2
```
zypper ar http://download.opensuse.org/repositories/home:/gpaterno:/SecurePass/openSUSE_13.2/home:gpaterno:SecurePass.repo
```

## Sources

Most of the sources are available on github https://github.com/garlsecurity


# Configure on Linux

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

An automated configuration script is available in the securepass tools and is named **sp-config**.
This will allow to fully automate the deploy of securepass in any environment, ex: kickstart, preseed, 
cloud-init (OpenStack) or from your favorite automation tool (ansible, puppet, chef, ...)

The syntax is:

```
sp-config -i <<YOUR APPLICATION ID>> -s  <<YOUR APPLICATION SECRET>> -r login.farm 
```

an optional **--root** is available to automatically populate root ssh keys with the
list of users (coma separated).


# PAM/NSS Module

Unix identities can be looked up in login.farm through the NSS module, so that you can have a 
consistant UID, GID, shell and home directory across all your instances. The PAM module can be 
setup so that the login.farm will handle the authentication in your Linux system, leveraging the 
built-in 2FA of login.farm.





# SSH Configuration

The securepass tools have an openssh filter that can be used to automatically retrive
users' ssh keys from login.farm. First of all, install your securepass tools via RPM/deb
or **pip install securepass**.

Then configure your sshd_config by adding the following line:

```
AuthorizedKeysCommand /usr/bin/sp-sshkey
AuthorizedKeysCommandUser nobody
```

The openssh helper is able to generate authorized keys for root user if you specify in
/etc/securepass.conf the following:

```
[ssh]

root = user1,user2
```

This will automatically allow user1 and user2 keys to login as root.

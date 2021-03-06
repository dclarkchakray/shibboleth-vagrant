# Shibboleth-vagrant
Vagrant box - Shibboleth IdP3 + SP 2.x testing environment

Shibboleth SP / IdP are almost on factory settings. ~~So IdP wont release any attributes to the SP.~~ IdP will release couple of attributes to the Shibboleth SP (uid, mail, sn, cn, givenName)

## Prereqs
- Vagrant 2.0.x ([https://www.vagrantup.com/]())
- Virtual Box 5.2.x ([https://www.virtualbox.org/]())

## This Vagrant box includes following packages / applications:
* CentOS 6.4
* httpd 2.2
* Oracle Java 8
* Tomcat 8.0.30
* Shibboleth Service Provider (SP) 2.5.x
* Shibboleth Identity Provider (IdP) 3.2.0
* OpenLDAP
* phpLdapAdmin

+ Google Authenticator module:  
https://github.com/korteke/Shibboleth-IdP3-TOTP-Auth  

All programs are provisioned to vagrant box with [Ansible](https://www.google.com)

# Installation

Install the Vagrant Nugrant Plugin

vagrant plugin install nugrant

Copy .vagrantuser_dist to .vagrantuser and update to your local values

# Configuration

You need to add Vagrant box ip address to the hosts-file (linux /etc/hosts, windows c:\windows\system32\drivers\etc\hosts)

Change this value to your hostname as defined in the .vagrantuser, by default this uses vagrant.local in the following steps replace this with your hostname

```
192.168.0.120 vagrant.local
```

# Usage

* Execute "vagrant up" and wait that the ansible run has completed, expected outcome:

```
PLAY RECAP ********************************************************************
default                    : ok=50   changed=48   unreachable=0    failed=0
```

Open browser and navigate to the address "https://vagrant.local/secure/" this URL is secured with Shibboleth SP, so that will redirect you to the Shibboleth IdP where you need to authenticate.

You can use following users to test this setup:
* johnd / Password1
* janed / Password1  

Google Authenticator flow can be tested with URL: https://vagrant.local/Shibboleth.sso/totp  or https://vagrant.local/Shibboleth.sso/totp?target=/secure  
Latter URL will redirect you to the so simple PHP-site where you can see your attributes & headers.

Google Authenticator seed  for "johnd" = G24YUKCHHXRDWCPR  
QR-code:  
![alt text](https://kvak.net/totp_code_qr.png "Logo Title Text 1")


After authentication you will be redirected back to https://vagrant.local/secure/. There is a simple PHP site which will show your environment variables and http headers.

# Manage
You can use phpLdapAdmin application to manage users that are allowed to authenticate. It can be found https://vagrant.local/ldapadmin. Authenticate with user: "cn=manager,dc=vagrant,dc=local" password: "Password1"

# Debugging LDAP
Ldap debugging is on by default has been enabled and the log files are available in the host:

$>vagrant ssh
$>tail -f /var/log/ldap.log

To see an individual request emptying out the existing log file:

$>cat /dev/null > /var/log/ldap.log

Then action the request and review the file as there will be a large amount of debug information.

# File locations

* Shibboleth IdP - /opt/shibboleth-idp
* Shibboleth SP - /etc/shibboleth
* Apache httpd - /etc/httpd
* Java - /opt/jdk1.8.0_45/
* Tomcat - /opt/apache-tomcat-8.0.30
* OpenLDAP - /etc/openldap

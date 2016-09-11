apache
==================

[![Build Status](https://travis-ci.org/vbotka/ansible-apache.svg?branch=master)](https://travis-ci.org/vbotka/apache)

Ansible role. Install and configure apache on FreeBSD.

https://galaxy.ansible.com/vbotka/ansible-apache/

Tested with FreeBSD 10.3 at [digitalocean.com](https://cloud.digitalocean.com)


Requirements
------------

No requiremenst.


Variables
---------

By default SSL is off.

```
apache_ssl: "no"
```

Certificates are needed for SSL..

```
apache_ssl: "yes"
apache_SSLCertificateFile: "/usr/local/etc/apache24/server.crt"
apache_SSLCertificateKeyFile: "/usr/local/etc/apache24/server.key"
```

apache_vhost: Virtual hosts are configured with manadtory SSL. Both
80/443 virtual hosts will be created and 80 permanently redirected to
443. SEE example in vars.

TBD (Check the defaults).


Workflow
--------

1) Change shell to /bin/sh.

```
ansible mailserver -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
```

2) Install role.

```
ansible-galaxy install vbotka.ansible-apache
```

3) Fit variables.

```
~/.ansible/roles/vbotka.ansible-apache/vars/main.yml
```

4) Create playbook and inventory.

```
> cat ~/.ansible/playbooks/apache.yml
---
- hosts: webserver
  become: yes
  become_method: sudo
  roles:
    - role: vbotka.ansible-apache
```

```
> cat ~/.ansible/hosts
[webserver]
<MAILSERVER-IP-OR-FQDN>

[mailserver:vars]
ansible_connection=ssh
ansible_user=freebsd
ansible_python_interpreter=/usr/local/bin/python2
ansible_perl_interpreter=/usr/local/bin/perl
```

5) Install and configure apache.

```
ansible-playbook ~/.ansible/playbooks/apache.yml
```

6) Consider to test the webserver with:

   - http://validator.w3.org
   - https://www.ssllabs.com
		

License
-------

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


Author Information
------------------

[Vladimir Botka](https://botka.link)

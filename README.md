apache
==================

[![Build Status](https://travis-ci.org/vbotka/ansible-apache.svg?branch=master)](https://travis-ci.org/vbotka/apache)

[Ansible role.](https://galaxy.ansible.com/vbotka/apache/) Install and configure Apache.


Requirements
------------

No requiremenst.


Variables
---------

By default SSL is off. Check other defaults and examples in vars.

```
apache_ssl: False
```

Certificates are needed to enable SSL.

```
apache_ssl: True
apache_version: "24"
apache_SSLCertificateFile: "/usr/local/etc/apache{{ apache_version }}/server.crt"
apache_SSLCertificateKeyFile: "/usr/local/etc/apache{{ apache_version }}/server.key"
```

Virtual hosts are configured with mandatory SSL. Virtual hosts for
ports 80 and 443 will be created and port 80 permanently redirected to
443. Example is available in vars.

Example of variable apache_vhost:

```
apache_vhost:
  - { ServerName: "example.net",
      DocumentRoot: "/usr/local/www/apache24/data/example.net/",
      SSLCertificateFile: "/usr/local/etc/letsencrypt/live/example.net/fullchain.pem",
      SSLCertificateKeyFile: "/usr/local/etc/letsencrypt/live/example.net/privkey.pem"
      }                                                                                                                      
```

Example of a virtual host's configuration file:

```
# cat /usr/local/etc/apache24/extra/example.net.conf
<VirtualHost *:80>
ServerName example.net
DocumentRoot /usr/local/www/apache24/data/example.net/
Redirect permanent / https://example.net/
</VirtualHost>

<VirtualHost *:443>
ServerName example.net
DocumentRoot /usr/local/www/apache24/data/example.net/
SSLCertificateFile /usr/local/etc/letsencrypt/live/example.net/fullchain.pem
SSLCertificateKeyFile /usr/local/etc/letsencrypt/live/example.net/privkey.pem
</VirtualHost>
```


**Defaults**

```
apache_enable: True
apache_version: "24"
apache_ssl: False
apache_vhost: []
apache_conf_path: "/usr/local/etc/apache24"

apache_ServerName: "www.example.com"
apache_ServerAdmin: "admin@example.com"

apache_SSLCompression: "off"
apache_SSLCertificateFile: "/usr/local/etc/apache24/server.crt"
apache_SSLCertificateKeyFile: "/usr/local/etc/apache24/server.key"
apache_SSLProtocol: "all -SSLv2 -SSLv3 -TLSv1 -TLSv1.1"
apache_SSLCipherSuite: "ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256"
```


Workflow
--------

1) Change shell to /bin/sh.

```
# ansible webserver -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
```

2) Install role.

```
# ansible-galaxy install vbotka.apache
```

3) Fit variables.

```
# editor vbotka.apache/vars/main.yml
```

4) Create playbook and inventory.

```
# cat apache.yml
---
- hosts: webserver
  roles:
    - vbotka.apache
```

```
# cat hosts
[webserver]
<webserver-ip-or-fqdn>
[webserver:vars]
ansible_connection=ssh
ansible_user=freebsd
ansible_become=yes
ansible_become_method=sudo
ansible_python_interpreter=/usr/local/bin/python2
ansible_perl_interpreter=/usr/local/bin/perl
```

5) Install and configure apache.

```
# ansible-playbook apache.yml
```

6) Consider to test the webserver

   - http://validator.w3.org
   - https://www.ssllabs.com
		

References
----------

- [Apache HTTP Server Documentation](https://httpd.apache.org/docs/)
- [SSL/TLS Strong Encryption: Trunk: How-To](https://httpd.apache.org/docs/trunk/ssl/ssl_howto.html)
- [SSL/TLS Strong Encryption: 2.4: How-To](https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html)
- [SSL with Virtual Hosts Using SNI](https://wiki.apache.org/httpd/NameBasedSSLVHostsWithSNI)
- [Multi-Processing Modules (MPMs)](https://httpd.apache.org/docs/2.4/mpm.html)

License
-------

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


Author Information
------------------

[Vladimir Botka](https://botka.link)

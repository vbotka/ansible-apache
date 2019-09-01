# apache

[![Build Status](https://travis-ci.org/vbotka/ansible-apache.svg?branch=master)](https://travis-ci.org/vbotka/ansible-apache)

[Ansible role.](https://galaxy.ansible.com/vbotka/apache/) Install and configure Apache.


## Requirements

- [config_encoder_filters](https://galaxy.ansible.com/jtyr/config_encoder_filters)
- [ansible_lib](https://galaxy.ansible.com/vbotka/ansible_lib)


## Variables

Review defaults and examples in vars. By default SSL is off.

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

Virtual hosts are configured with optional redirection to SSL. By
default virtual hosts for ports 80 and 443 will be created and port 80
permanently redirected to 443. Example is available in vars.


## Workflow

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
ansible_python_interpreter=/usr/local/bin/python3.6
ansible_perl_interpreter=/usr/local/bin/perl
```

5) Install and configure apache.

```
# ansible-playbook apache.yml
```

6) Syntax check.

It's a good idea to run the playbook with "--syntax-check" first to
see potential problems. But, before running the playbook it's
necessary to install required packages (see
[defaults](https://github.com/vbotka/ansible-apache/tree/master/vars/defaults)). Otherwise
the role will complain about missing configuration files.

7) Consider to test the webserver.

   - http://validator.w3.org
   - https://www.ssllabs.com
		

## References

- [Apache HTTP Server Documentation](https://httpd.apache.org/docs/)
- [SSL/TLS Strong Encryption: Trunk: How-To](https://httpd.apache.org/docs/trunk/ssl/ssl_howto.html)
- [SSL/TLS Strong Encryption: 2.4: How-To](https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html)
- [SSL with Virtual Hosts Using SNI](https://wiki.apache.org/httpd/NameBasedSSLVHostsWithSNI)
- [Multi-Processing Modules (MPMs)](https://httpd.apache.org/docs/2.4/mpm.html)
- [FreebSD handbook: 29.8. Apache HTTP Server](https://www.freebsd.org/doc/handbook/network-apache.html)


## License

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


## Author Information

[Vladimir Botka](https://botka.link)

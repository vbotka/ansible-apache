Annotated source code
*********************

Ansible role `apache <https://galaxy.ansible.com/vbotka/apache/>`_ 

Overview
========


Tree
----
::

    $ tree .
    .
    ├── defaults
    │   └── main.yml
    ├── handlers
    │   └── main.yml
    ├── LICENSE
    ├── meta
    │   └── main.yml
    ├── README.md
    ├── requirements.yml
    ├── tasks
    │   ├── debug.yml
    │   ├── fn
    │   │   └── httpd-confd-vhost-dirs.yml
    │   ├── httpd-alias.yml
    │   ├── httpd-confd-includes.yml
    │   ├── httpd-confd-vhosts.yml
    │   ├── httpd-confd.yml
    │   ├── httpd-dirs.yml
    │   ├── httpd-modules.yml
    │   ├── httpd-ssl.yml
    │   ├── httpd-vhosts.yml
    │   ├── httpd.yml
    │   ├── main.yml
    │   ├── packages-freebsd.yml
    │   ├── packages.yml
    │   ├── rcconf.yml
    │   ├── service.yml
    │   └── vars.yml
    ├── templates
    │   ├── directory-block.j2
    │   ├── section2.j2
    │   ├── vhost2.j2
    │   └── vhost.j2
    ├── tests
    │   ├── inventory
    │   └── test.yml
    └── vars
        ├── conf.d
        │   ├── sections
        │   ├── sections-sample
        │   │   └── usr-local-www-example.com.yml
        │   ├── vhosts
        │   └── vhosts-sample
        │       └── example.com.yml
        ├── defaults
        │   ├── default.yml
        │   └── FreeBSD.yml
        ├── default.yml
        └── main.yml

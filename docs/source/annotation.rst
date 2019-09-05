Annotated source code
*********************

Ansible role `apache <https://galaxy.ansible.com/vbotka/apache/>`_ 

Overview
========

Tags
----
::

    $ ansible-playbook apache.yml --list-tags
    
    playbook: apache.yml
    
    play #1 (srv.example.conf): srv.example.com	TAGS: []
      TASK TAGS: [always, apache-debug, apache-httpd, apache-httpd-alias,
      apache-httpd-confd, apache-httpd-confd-includes, apache-httpd-confd-vhosts,
      apache-httpd-dirs, apache-httpd-modules, apache-httpd-ssl, apache-httpd-vhosts,
      apache-packages, apache-service, apache-vars]

To see the list of the variables only use the tag apache-debug
::
    
    $ ansible-playbook apache.yml -t apache_debug -e 'apache_debug=true'


To check what packages will be installed run
::
    
    $ ansible-playbook apache.yml -t apache_packages -e 'apache_debug=true' --check

Install packages only
::

    $ ansible-playbook apache.yml -t apache_packages -e 'apache_debug=true'


Debug
-----

To see additional debug information in the output enable debug output
in the configuration
::

    apache_debug: true

, or set the extra variable in the command
::

    $ ansible-playbook apache.yml -e 'apache_debug=true'


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

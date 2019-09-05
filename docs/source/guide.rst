User's guide
************

Introduction
============

The role install and configure Apache web server.


Requirements
------------

* Ansible role: `Apache <https://galaxy.ansible.com/vbotka/apache/>`_
* Supported systems: FreeBSD
* Requirements:
 
    * `ansible_lib <https://galaxy.ansible.com/vbotka/ansible_lib>`_
    * `config_encoder_filters <https://galaxy.ansible.com/jtyr/config_encoder_filters>`_


Installation
============

Take a look at the current status of the role
::

    $ ansible-galaxy info vbotka.apache

and install it
::

    $ ansible-galaxy install vbotka.apache

Together with the role ``vbotka.apache`` two other roles will be
installed. The role ``vbotka.ansible_lib`` provides some common tasks in
the form of included tasks and the role
``jtyr.config_encoder_filters`` provides the filter ``encode_apache``
used to encode YAML configuration data to Apache format.

Take a look at other roles
::

    $ ansible-galaxy search --author=vbotka


For details how to install specific versions from various sources see
`Installing content
<https://galaxy.ansible.com/docs/using/installing.html>`__.


Ansible playbook
================

Simple playbook to install and configure Apache at srv.example.com
::

     $ cat apache.yml

     - hosts: srv.example.com
       connection: ssh
       remote_user: admin
       become: yes
       become_user: root
       become_method: sudo
       roles:
         - vbotka.apache

For details see `Connection Plugins
<https://docs.ansible.com/ansible/latest/plugins/connection.html>`__
and `Understanding Privilege Escalation
<https://docs.ansible.com/ansible/latest/user_guide/become.html#understanding-privilege-escalation>`__.


Tags
====

The tags provide very useful tool to run selected tasks of the
role. List the available tags of the role.

::

    $ ansible-playbook apache.yml --list-tags

    playbook: apache.yml

    play #1 (srv.example.conf): srv.example.com	TAGS: []
      TASK TAGS: [always, apache-debug, apache-httpd, apache-httpd-alias,
      apache-httpd-confd, apache-httpd-confd-includes, apache-httpd-confd-vhosts,
      apache-httpd-dirs, apache-httpd-modules, apache-httpd-ssl, apache-httpd-vhosts,
      apache-packages, apache-service, apache-vars]

For example see the list of the variables with the tag apache-debug
::

    $ ansible-playbook apache.yml -t apache_debug -e 'apache_debug=true'


See what packages will be installed
::

    $ ansible-playbook apache.yml -t apache_packages -e 'apache_debug=true' --check

Install packages only and exit the play. Enable the debug output
::

    $ ansible-playbook apache.yml -t apache_packages -e 'apache_debug=true'


Debug
=====

To see additional debug information in the output enable debug output
in the configuration
::

    apache_debug: true

, or set the extra variable in the command
::

    $ ansible-playbook apache.yml -e 'apache_debug=true'


Variables
=========

Default variables
-----------------

Most of the variables are self-explaining (4-9,12-13,64-65). For Apache configuration (19-54,60)
see `Apache HTTP Server Documentation
<https://httpd.apache.org/docs/>`__. Other variables (70,73,76,79-80) will be explained
in the next sections.

.. highlight:: Yaml
    :linenothreshold: 5

`defaults/main.yml <https://github.com/vbotka/ansible-apache/blob/master/defaults/main.yml>`_

.. literalinclude:: ../../defaults/main.yml
    :language: Yaml
    :emphasize-lines: 4-9,12-13,64-65,70,73,76,79-80
    :linenos:



OS specific default variables
-----------------------------

Here come the OS specific default variables. The configuration files
in the directory ``vars/defaults`` will be included
``with_first_found`` (1). At least empty ``default.yml`` (6) shall be present.

.. code-block:: yaml
   :emphasize-lines: 1,6
   :linenos:

    with_first_found:
    - files:
        - "{{ ansible_distribution }}-{{ ansible_distribution_release }}.yml"
        - "{{ ansible_distribution }}.yml"
        - "{{ ansible_os_family }}.yml"
        - "default.yml"
        - "defaults.yml"
      paths: "{{ al_os_vars_path }}/vars/defaults"


FreeBSD default variables
^^^^^^^^^^^^^^^^^^^^^^^^^

By default the binary packages will be installed (4). But if custom
builds are available switch to ``ports`` (5) and use
``freebsd_use_packages: "yes"`` (6) to speedup the installation. Under
standard circumstances, there is no reason to change other parameters
here.

`vars/defaults/FreeBSD.yml <https://github.com/vbotka/ansible-apache/blob/master/vars/defaults/FreeBSD.yml>`_

.. literalinclude:: ../../vars/defaults/FreeBSD.yml
    :language: Yaml
    :emphasize-lines: 4-6
    :linenos:



OS specific custom variables
----------------------------

Here come the OS specific custom variables. The configuration files
in the directory ``vars`` will be included ``with_first_found`` (1) and
will override the default values of the variables. At least empty
``default.yml`` (6) shall be present here.

.. code-block:: yaml
   :emphasize-lines: 1,6
   :linenos:

    with_first_found:
    - files:
        - "{{ ansible_distribution }}-{{ ansible_distribution_release }}.yml"
        - "{{ ansible_distribution }}.yml"
        - "{{ ansible_os_family }}.yml"
        - "default.yml"
        - "defaults.yml"
      paths: "{{ al_os_vars_path }}/vars"


apache_vhost
------------

``apache_vhost`` is a list of virtual hosts. The example below will
configure virtual server ``example.net`` (2). For details see
`httpd-vhosts.yml
<https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-vhosts.yml>`__

.. code-block:: yaml
   :emphasize-lines: 2
   :linenos:

    apache_vhost:
      - ServerName: "mail.example.net"
        DocumentRoot: "/usr/local/www/roundcube/"
        SSLCertificateFile: /usr/local/etc/letsencrypt/live/mail.example.net/fullchain.pem
        SSLCertificateKeyFile: /usr/local/etc/letsencrypt/live/mail.example.net/privkey.pem

It is also possible to configure virtual servers with
``apache_confd_dir_vhosts``. The variables do not depend on each other
and can be used together.


apache_directory_blocks
-----------------------

``apache_directory_blocks`` is a list of virtual hosts'
``DocumentRoot`` directories (2) and related parameters
(4). Configuration file (3) will be created in the directory ``{{
apache_conf_path }}/Includes/``. For details see `httpd-dirs.yml
<https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-dirs.yml>`__.

.. code-block:: yaml
   :emphasize-lines: 2,3,4
   :linenos:

    apache_directory_blocks:
      - Directory: "/usr/local/www/roundcube"
        Includefile: "usr-local-www-roundcube.conf"
        Conf:
          - "Options Indexes FollowSymLinks"
          - "DirectoryIndex index.html"
          - "AllowOverride All"
          - "Require all granted"


apache_alias
------------

``apache_alias`` is a list of aliases. See example below. For details see `httpd-alias.yml
<https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-alias.yml>`__.

.. code-block:: yaml
   :linenos:

    apache_alias:
      - "ScriptAlias /nagios/cgi-bin/ /usr/local/www/nagios/cgi-bin/"
      - "Alias /nagios/ /usr/local/www/nagios/"
      - "Alias /joomla /usr/local/www/joomla3/"


apache_confd_dir_vhosts
-----------------------

``apache_confd_dir_vhosts`` is path to directory with virtual hosts'
configuration files. The format of the files is described in the
filter `encode_apache
<https://galaxy.ansible.com/jtyr/config_encoder_filters>`_.

The default value is
::

    apache_confd_dir_vhosts: "{{ role_path }}/vars/conf.d/vhosts"

In projects it might be convenient to change the path. For example
::

    apache_confd_dir_vhosts: "{{ playbook_dir }}/apache.d/vhosts"

For example from the configuration file below
::

    $ cat mail.example.net/apache.d/vhosts/mail.example.net.yml
    my_apache_vhost:
      content:
        - sections:
            - name: VirtualHost
              param: "*:80"
              content:
                - options:
                    - DocumentRoot: /usr/local/www/roundcube/
                    - ServerName: mail.example.net
                    - Redirect permanent /: https://mail.example.net/
        - sections:
            - name: VirtualHost
              param: "*:443"
              content:
                - options:
                    - ServerName: mail.example.net
                    - DocumentRoot: /usr/local/www/roundcube/
                    - SSLCertificateFile: /usr/local/etc/ssl/certs/mail.example.net.crt
                    - SSLCertificateKeyFile: /usr/local/etc/ssl/private/mail.example.net.key

the configuration file ``{{ apache_conf_path
}}/extra/mail.example.net.conf`` will be created and the file will be
included in ``{{ apache_conf_path }}/httpd.conf``

For details see `httpd-confd-vhosts.yml
<https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-confd-vhosts.yml>`__.


apache_confd_dir_sections
-------------------------

``apache_confd_dir_sections`` is path to directory with configuration
files. The format of the files is described in the filter
`encode_apache
<https://galaxy.ansible.com/jtyr/config_encoder_filters>`_. The
content of the files will be encoded and stored in the files in the
directory ``{{ apache_conf_path }}/Includes/``.

The default value is
::

    apache_confd_dir_sections: "{{ role_path }}/vars/conf.d/sections"

In case of project based directories' structure it might be convenient
to change the path. For example
::

    apache_confd_dir_sections: "{{ playbook_dir }}/apache.d/sections"

For example from the configuration file below
::

    $ cat mail.example.net/apache.d/sections/usr-local-www-roundcube.yml
    my_apache_dir:
      content:
        - sections:
            - name: Directory
              param: /usr/local/www/roundcube
              content:
                - options:
                    - Options:
                        - Indexes
                        - FollowSymLinks
                    - AllowOverride: All
                    - Require:
                        - all
                        - granted


the configuration file ``usr-local-www-roundcube.conf`` will be
created and stored in the directory ``{{ apache_conf_path
}}/Includes``
::

    $ cat /usr/local/etc/apache24/Includes/usr-local-www-roundcube.conf
    <Directory /usr/local/www/roundcube>
      Options Indexes FollowSymLinks
      AllowOverride All
      Require all granted
    </Directory>


For details see `httpd-confd-includes.yml
<https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-confd-includes.yml>`__.

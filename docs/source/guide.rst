.. _users_guide:

User's guide
************

.. contents:: Topics

Welcome to the Users's Guide!


.. _users_guide_introduction:

Introduction
============

The role install and configure Apache web server.


.. _users_guide_requirements:

Requirements
------------

* Ansible role: `Apache <https://galaxy.ansible.com/vbotka/apache/>`_
* Supported systems: `FreeBSD <https://www.freebsd.org/>`_
* Requirements:
 
    * `vbotka.ansible_lib <https://galaxy.ansible.com/vbotka/ansible_lib>`_
    * `jtyr.config_encoder_filters <https://galaxy.ansible.com/jtyr/config_encoder_filters>`_


.. _users_guide_installation:

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


.. seealso:: For details how to install specific versions from various
             sources see `Installing content
             <https://galaxy.ansible.com/docs/using/installing.html>`__.


.. _users_guide_ansible_playbook:

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


.. seealso:: For details see `Connection Plugins
             <https://docs.ansible.com/ansible/latest/plugins/connection.html>`__
             and `Understanding Privilege Escalation
             <https://docs.ansible.com/ansible/latest/user_guide/become.html#understanding-privilege-escalation>`__.


.. _users_guide_tags:

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


.. _users_guide_debug:

Debug
=====

To see additional debug information in the output enable debug output
in the configuration
::

    apache_debug: true

, or set the extra variable in the command
::

    $ ansible-playbook apache.yml -e 'apache_debug=true'


.. _users_guide_variables:

Variables
=========


.. _users_guide_defaults:

Default variables
-----------------

* Most of the variables are self-explaining (4-9,12-13,64-65)
* For Apache configuration (19-54,60) see `Apache HTTP Server Documentation <https://httpd.apache.org/docs/>`__.
* Other variables (70,73,76,79-80) will be explained in the next sections.

.. highlight:: Yaml
    :linenothreshold: 5

[`defaults/main.yml <https://github.com/vbotka/ansible-apache/blob/master/defaults/main.yml>`_]

.. literalinclude:: ../../defaults/main.yml
    :language: Yaml
    :emphasize-lines: 4-9,12-13,64-65,70,73,76,79-80
    :linenos:

.. warning:: By default SSL is turned off. See ``apache_SSLEngine: "off"`` (9).


.. _users_guide_os_defaults:

OS specific default variables
-----------------------------

Here come the OS specific default variables. The configuration files
in the directory ``vars/defaults`` will be included
``with_first_found`` (1). At least empty ``default.yml`` (6) shall be present.

* [`tasks/vars.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/vars.yml>`_]
* [`al_include_os_vars_path.yml  <https://github.com/vbotka/ansible-lib/blob/master/tasks/al_include_os_vars_path.yml>`_]

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

.. note:: OS specific variables are included witht he module
	   ``include_var``. This has very high precedence (18 in the
	   22 scale). See `Ansible variable precedence: Where should I
	   put a variable?
	   <https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable>`_
	   To override these variables see :ref:`users_guide_os_custom`


.. _users_guide_os_defaults_freebsd:

FreeBSD default variables
^^^^^^^^^^^^^^^^^^^^^^^^^

By default the binary packages will be installed (4). But if custom
builds are available switch to ``ports`` (5) and use
``freebsd_use_packages: "yes"`` (6) to speedup the installation. Under
standard circumstances, there is no reason to change other parameters
here.

[`vars/defaults/FreeBSD.yml <https://github.com/vbotka/ansible-apache/blob/master/vars/defaults/FreeBSD.yml>`_]

.. literalinclude:: ../../vars/defaults/FreeBSD.yml
    :language: Yaml
    :emphasize-lines: 4-6
    :linenos:


.. _users_guide_os_custom:

OS specific custom variables
----------------------------

Here come the OS specific custom variables. The configuration files
in the directory ``vars`` will be included ``with_first_found`` (1) and
will override the default values of the variables. At least empty
``default.yml`` (6) shall be present here.

* [`tasks/vars.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/vars.yml>`_]
* [`al_include_os_vars_path.yml  <https://github.com/vbotka/ansible-lib/blob/master/tasks/al_include_os_vars_path.yml>`_]

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

.. _users_guide_apache_vhost:

apache_vhost
------------

``apache_vhost`` is a list of virtual hosts. The example below will
configure virtual server ``mail.example.net`` (2).

.. code-block:: yaml
   :emphasize-lines: 2
   :linenos:

    apache_vhost:
      - ServerName: "mail.example.net"
        DocumentRoot: "/usr/local/www/roundcube/"
        SSLCertificateFile: /usr/local/etc/letsencrypt/live/mail.example.net/fullchain.pem
        SSLCertificateKeyFile: /usr/local/etc/letsencrypt/live/mail.example.net/privkey.pem

.. seealso:: For details see :ref:`vhosts`. [`httpd-vhosts.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-vhosts.yml>`__]

.. note:: It is also possible to configure virtual servers with
          ``apache_confd_dir_vhosts``. The variables do not depend on
          each other and can be used together. See
          :ref:`users_guide_apache_confd_dir_vhosts`.

.. _users_guide_apache_directory_blocks:

apache_directory_blocks
-----------------------

``apache_directory_blocks`` is a list of virtual hosts'
``DocumentRoot`` directories (2) and related parameters
(4). Configuration file (3) will be created in the directory ``{{
apache_conf_path }}/Includes/``.

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

.. seealso:: For details see :ref:`includes`. [`httpd-dirs.yml
             <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-dirs.yml>`__]

.. _users_guide_apache_alias:

apache_alias
------------

``apache_alias`` is a list of aliases. See example below.

.. code-block:: yaml
   :linenos:

    apache_alias:
      - "ScriptAlias /nagios/cgi-bin/ /usr/local/www/nagios/cgi-bin/"
      - "Alias /nagios/ /usr/local/www/nagios/"
      - "Alias /joomla /usr/local/www/joomla3/"

.. seealso:: For details see :ref:`alias`. [`httpd-alias.yml
             <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-alias.yml>`__]


.. _users_guide_apache_confd_dir_vhosts:

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
included in ``{{ apache_conf_path }}/httpd.conf``.

.. seealso:: For details see
             :ref:`confd-vhosts`. [`httpd-confd-vhosts.yml
             <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-confd-vhosts.yml>`__]


.. _users_guide_apache_confd_dir_sections:

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
}}/Includes``.
::

    $ cat /usr/local/etc/apache24/Includes/usr-local-www-roundcube.conf
    <Directory /usr/local/www/roundcube>
      Options Indexes FollowSymLinks
      AllowOverride All
      Require all granted
    </Directory>


.. seealso:: For details see
             :ref:`confd-includes`. [`httpd-confd-includes.yml
             <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-confd-includes.yml>`__]

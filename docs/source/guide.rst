.. _ug:

User's guide
************
.. contents:: Topics
   :depth: 3

Welcome to the User's Guide!


.. _ug_introduction:

Introduction
============

The role install and configure Apache web server

* Ansible role: `Apache <https://galaxy.ansible.com/vbotka/apache/>`_
* Supported systems: `FreeBSD <https://www.freebsd.org/>`_
* Requirements:

    * `vbotka.ansible_lib <https://galaxy.ansible.com/vbotka/ansible_lib>`_
    * `jtyr.config_encoder_filters <https://galaxy.ansible.com/jtyr/config_encoder_filters>`_

The user of this role is expected to have read at least the following documents

* `Basic Concepts <https://docs.ansible.com/ansible/latest/network/getting_started/basic_concepts.html>`_
* `Roles <https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html>`_
* `Working With Playbooks <https://docs.ansible.com/ansible/latest/user_guide/playbooks.html>`_


.. _ug_installation:

Installation
============

The most convenient way how to install an Ansible role is to use
Ansible Galaxy CLI ``ansible-galaxy``. The utility comes with the
standard Ansible packages and provide the user with simple interface
to the Ansible Galaxy's services. For example take a look at the
current status of the role ::

    $ ansible-galaxy info vbotka.apache

and install it ::

    $ ansible-galaxy install vbotka.apache

Together with the role ``vbotka.apache`` two other roles will be
installed. The role ``vbotka.ansible_lib`` provides some common tasks
in the form of included tasks and the role
``jtyr.config_encoder_filters`` provides the filter ``encode_apache``
used to encode YAML configuration data to Apache format.

Take a look at other roles ::

    $ ansible-galaxy search --author=vbotka


.. seealso:: For details how to install specific versions from various
             sources see `Installing content
             <https://galaxy.ansible.com/docs/using/installing.html>`__.


.. _ug_ansible_playbook:

Ansible playbook
================

Simple playbook to install and configure Apache at srv.example.com (2)

.. code-block:: yaml
   :emphasize-lines: 2
   :linenos:

   $ cat apache.yml
   - hosts: srv.example.com
     gather_facts: true
     connection: ssh
     remote_user: admin
     become: yes
     become_user: root
     become_method: sudo
     roles:
       - vbotka.apache

.. note:: * | ``gather_facts: true`` (3) must be set to collect variables
            | needed to evaluate :ref:`ug_os_defaults` and :ref:`ug_os_custom`
	    | [``ansible_distribution``, ``ansible_distribution_release``, ``ansible_os_family``]
	  * | See `tasks/vars.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/vars.yml>`_

.. seealso:: * For details see `Connection Plugins
               <https://docs.ansible.com/ansible/latest/plugins/connection.html>`__ (4-5)
             * and `Understanding Privilege Escalation
               <https://docs.ansible.com/ansible/latest/user_guide/become.html#understanding-privilege-escalation>`__ (6-8).


.. _ug_tags:

Tags
====

The tags provide very useful tool to run selected tasks of the
role. To see what tags are available list the tags of the role with
command below

.. code-block:: yaml
   :emphasize-lines: 1
   :linenos:

    $ ansible-playbook apache.yml --list-tags

    playbook: apache.yml

    play #1 (srv.example.conf): srv.example.com	TAGS: []
      TASK TAGS: [always, apache-debug, apache-httpd, apache-httpd-alias,
      apache-httpd-confd, apache-httpd-confd-includes, apache-httpd-confd-vhosts,
      apache-httpd-dirs, apache-httpd-modules, apache-httpd-ssl, apache-httpd-vhosts,
      apache-packages, apache-service, apache-vars]

For example see the list of the variables and their values with the
tag apache-debug ::

    $ ansible-playbook apache.yml -t apache_debug -e 'apache_debug=true'

See what packages will be installed ::

    $ ansible-playbook apache.yml -t apache_packages -e 'apache_debug=true' --check

Install packages only and exit the play. Enable the debug output ::

    $ ansible-playbook apache.yml -t apache_packages -e 'apache_debug=true'


.. _ug_debug:

Debug
=====

To see additional debug information in the output enable debug output
in the configuration ::

    apache_debug: true

, or set the extra variable in the command ::

    $ ansible-playbook apache.yml -e 'apache_debug=true'

.. seealso:: * `Playbook Debugger <https://docs.ansible.com/ansible/latest/user_guide/playbooks_debugger.html>`_


.. _ug_variables:

Variables
=========

In this guide we describe role defaults variables in the directory ``defaults`` and
variables included from the directory ``vars``.

* role defaults in the directory ``{{ role_path }}/defaults``
  (precedence 2.)
* include OS specific vars from the directory ``{{ role_path }}/vars``
  (precedence 18.)

.. seealso:: `Ansible variable precedence: Where should I put a variable?
             <https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable>`_


.. _ug_defaults:

Default variables
-----------------

* Most of the variables are self-explaining (4-9,12-13,64-65)
* For Apache configuration (19-54,60) see `Apache HTTP Server
  Documentation <https://httpd.apache.org/docs/>`__.
* Other variables (70,73,76,79-80) will be explained in the next sections.

[`defaults/main.yml <https://github.com/vbotka/ansible-apache/blob/master/defaults/main.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../defaults/main.yml
    :language: yaml
    :emphasize-lines: 4-9,12-13,64-65,70,73,76,79-80
    :linenos:

.. warning:: By default SSL is turned off ``apache_SSLEngine: "off"`` (9).


.. _ug_os_defaults:

OS specific default variables
-----------------------------

Here come the OS specific default variables. The configuration files
in the directory ``vars/defaults`` will be included
``with_first_found`` (1). At least empty ``default.yml`` (6) shall be present.

* [`tasks/vars.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/vars.yml>`_]
* [`al_include_os_vars_path.yml
  <https://github.com/vbotka/ansible-lib/blob/master/tasks/al_include_os_vars_path.yml>`_]

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

.. note:: * OS specific variables are included with the module
	    ``include_var``. This has very high precedence (18 in the
	    22 scale).
          * See `Ansible variable precedence: Where should I put a
	    variable?
	    <https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable>`_
	  * To override these variables see :ref:`ug_os_custom`


.. _ug_os_defaults_freebsd:

FreeBSD default variables
^^^^^^^^^^^^^^^^^^^^^^^^^

By default the binary packages will be installed (4). But if custom
builds are available switch to ``ports`` (5) and use
``freebsd_use_packages: "yes"`` (6) to speedup the installation. Under
standard circumstances, there is no reason to change other parameters
here.

[`vars/defaults/FreeBSD.yml <https://github.com/vbotka/ansible-apache/blob/master/vars/defaults/FreeBSD.yml>`_]

.. literalinclude:: ../../vars/defaults/FreeBSD.yml
    :language: yaml
    :emphasize-lines: 4-6
    :linenos:


.. _ug_os_custom:

OS specific custom variables
----------------------------

Here come the OS specific custom variables. The configuration files
in the directory ``vars`` will be included ``with_first_found`` (1) and
will override the default values of the variables. At least empty
``default.yml`` (6) shall be present here.

* [`tasks/vars.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/vars.yml>`_]
* [`al_include_os_vars_path.yml
  <https://github.com/vbotka/ansible-lib/blob/master/tasks/al_include_os_vars_path.yml>`_]

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

.. note:: * OS specific variables from the directory ``{{ al_os_vars_path }}/vars`` override OS specific default variables from the directory ``{{ al_os_vars_path }}/vars/defaults``.
	  * See `al_include_os_vars_path.yml <https://github.com/vbotka/ansible-lib/blob/master/tasks/al_include_os_vars_path.yml>`_.


.. _ug_apache_vhost:

apache_vhost
------------
.. highlight:: yaml
.. contents::
   :local:

Synopsis
^^^^^^^^
* ``apache_vhost`` is a list of virtual hosts.

Parameters
^^^^^^^^^^
============================= ==================== ============================
| *Parameter*                 | *Type*             | *Comments*
============================= ==================== ============================
| **ServerName**              | *string*           | Fully qualified domain
                              | ``required``       | name (FQDN)
| **DocumentRoot**            | *string*           | Path DocumentRoot
                              | ``required``
| **SSLCertificateFile**      | *string*           | Path to SSL Certificate
                              | ``required``
| **SSLCertificateKeyFile**   | *string*           | Path to SSL Private key
                              | ``required``
| **redirect**                | *boolean*          | Redirect permanent http
                              | ``default: false`` | to https
| **create_document_root**    | *boolean*          | Create DocumentRoot
                              | ``default: false``
============================= ==================== ============================

Example
^^^^^^^
The example below will configure virtual server ``mail.example.net`` (2).

.. code-block:: yaml
   :emphasize-lines: 2
   :linenos:

   apache_vhost:
     - ServerName: "mail.example.net"
       DocumentRoot: "/usr/local/www/roundcube/"
       SSLCertificateFile: "/usr/local/etc/letsencrypt/live/mail.example.net/fullchain.pem"
       SSLCertificateKeyFile: "/usr/local/etc/letsencrypt/live/mail.example.net/privkey.pem"

Notes
^^^^^
.. note:: * The default value is an empty dictionary ``apache_vhost: {}``
	  * For details see :ref:`as_vhosts`. [`httpd-vhosts.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-vhosts.yml>`_]

See Also
^^^^^^^^
.. seealso:: * It is also possible to configure virtual servers with ``apache_confd_dir_vhosts``. See :ref:`ug_apache_confd_dir_vhosts`.


.. _ug_apache_directory_blocks:

apache_directory_blocks
-----------------------
.. highlight:: yaml
.. contents::
   :local:

Synopsis
^^^^^^^^
* ``apache_directory_blocks`` is a list of directory blocks.

Parameters
^^^^^^^^^^
============================= ============== ===================================
| *Parameter*                 | *Type*       | *Comments*
============================= ============== ===================================
| **Directory**               | *string*     | DocumentRoot directory
                              | ``required``
| **Includefile**             | *string*     | Path to the includefile to be
                              | ``required`` | created
| **Conf**                    | *list*       | Configuration of the directory
============================= ============== ===================================

Example
^^^^^^^
Configuration file (3) will be created in the directory ``{{ apache_conf_path }}/Includes/``.

.. code-block:: yaml
   :emphasize-lines: 3
   :linenos:

    apache_directory_blocks:
      - Directory: "/usr/local/www/roundcube"
        Includefile: "usr-local-www-roundcube.conf"
        Conf:
          - "Options Indexes FollowSymLinks"
          - "DirectoryIndex index.html"
          - "AllowOverride All"
          - "Require all granted"

Notes
^^^^^
.. note:: * The default value is an empty dictionary ``apache_directory_blocks: {}``
	  * For details see :ref:`as_includes`. [`httpd-dirs.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-dirs.yml>`_]

See Also
^^^^^^^^
.. seealso:: * :ref:`as_directory-block`


.. _ug_apache_alias:

apache_alias
------------
.. highlight:: yaml
.. contents::
   :local:

Synopsis
^^^^^^^^
* ``apache_alias`` is a list of aliases.

Example
^^^^^^^
.. code-block:: yaml
   :linenos:

    apache_alias:
      - "ScriptAlias /nagios/cgi-bin/ /usr/local/www/nagios/cgi-bin/"
      - "Alias /nagios/ /usr/local/www/nagios/"
      - "Alias /joomla /usr/local/www/joomla3/"

Notes
^^^^^
.. note:: * The default value is an empty list ``apache_alias: []``
	  * For details see :ref:`as_alias`. [`httpd-alias.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-alias.yml>`_]


.. _ug_apache_confd_dir_vhosts:

apache_confd_dir_vhosts
-----------------------
.. highlight:: yaml
.. contents::
   :local:

Synopsis
^^^^^^^^
* ``apache_confd_dir_vhosts`` is path to directory with virtual hosts'
  configuration files.

Parameters
^^^^^^^^^^
The parameters and format of the files are described in the filter
`encode_apache
<https://github.com/jtyr/ansible-config_encoder_filters#id6>`_.

Example
^^^^^^^
From the configuration file below the configuration file ``{{
apache_conf_path }}/extra/mail.example.net.conf`` will be created and
the file will be included in ``{{ apache_conf_path }}/httpd.conf``.

.. code-block:: yaml
   :emphasize-lines: 3
   :linenos:

    $ cat mail.example.net/apache.d/vhosts/mail.example.net.yml
    my_apache_vhost:
      content:
        - sections:
            - name: VirtualHost
              param: "*:80"
              content:
                - options:
                    - ServerName: mail.example.net
                    - DocumentRoot: /usr/local/www/roundcube/
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

Notes
^^^^^
.. note:: * For details see :ref:`as_confd-vhosts`. [`httpd-confd-vhosts.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-confd-vhosts.yml>`_]

Hints
^^^^^
.. hint:: * | The default value is
            | ``apache_confd_dir_vhosts: "{{ role_path }}/vars/conf.d/vhosts"``
          * | In projects it might be convenient to change the path. For example
            | ``apache_confd_dir_vhosts: "{{ playbook_dir }}/apache.d/vhosts"``


.. _ug_apache_confd_dir_sections:

apache_confd_dir_sections
-------------------------
.. highlight:: yaml
.. contents::
   :local:

Synopsis
^^^^^^^^
* ``apache_confd_dir_sections`` is path to directory with
  configuration files.

Parameters
^^^^^^^^^^
The parameters and format of the files are described in the filter
`encode_apache
<https://github.com/jtyr/ansible-config_encoder_filters#id6>`_. The
content of the files will be encoded and stored in the files in the
directory ``{{ apache_conf_path }}/Includes/``.

Example
^^^^^^^
For example from the configuration file below the configuration file
``usr-local-www-roundcube.conf`` will be created and stored in the
directory ``{{ apache_conf_path }}/Includes`` (17).

.. code-block:: yaml
   :emphasize-lines: 5-6,9-15,17
   :linenos:

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

   $ cat /usr/local/etc/apache24/Includes/usr-local-www-roundcube.conf
   <Directory /usr/local/www/roundcube>
     Options Indexes FollowSymLinks
     AllowOverride All
     Require all granted
   </Directory>

Notes
^^^^^
.. note:: * For details see :ref:`as_confd-includes`. [`httpd-confd-includes.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-confd-includes.yml>`_]

Hints
^^^^^
.. hint:: * | The default value is
            | ``apache_confd_dir_vhosts: "{{ role_path }}/vars/conf.d/sections"``
          * | In projects it might be convenient to change the path. For example
            | ``apache_confd_dir_vhosts: "{{ playbook_dir }}/apache.d/sections"``


.. _ug_apache_httpd_conf:

apache_httpd_conf
-----------------
.. highlight:: yaml
.. contents::
   :local:

Synopsis
^^^^^^^^
* ``apache_httpd_conf`` is a list of lines in httpd.conf.

Parameters
^^^^^^^^^^
============================= ==================== =============================
| *Parameter*                 | *Type*             | *Comments*
============================= ==================== =============================
| **regexp**                  | *string*           | The pattern to replace if
                              | ``required``       | found
| **line**                    | *string*           | The line to insert/replace
                              | ``required``       | into the file
============================= ==================== =============================

Example
^^^^^^^
.. code-block:: yaml
   :linenos:

   apache_httpd_conf:
     - {regexp: "ServerName", line: "{{ apache_ServerName }}"}
     - {regexp: "ServerAdmin", line: "{{ apache_ServerAdmin }}"}
     - {regexp: "ServerRoot", line: "/usr/local"}
     - {regexp: "MIMEMagicFile", line: "etc/apache24/magic"}

Notes
^^^^^
.. note:: | * The default value is
	  |   ``apache_httpd_conf:``
          |     ``- {regexp: "ServerName", line: "{{ apache_ServerName }}"}``
	  |     ``- {regexp: "ServerAdmin", line: "{{ apache_ServerAdmin }}"}``
	  | * The argument line must be quoted if it contains spaces
	  |     ``- {regexp: "ErrorDocument 500", line: "\"The server made a boo boo.\""}``
          | * For details see :ref:`as_httpd-conf`. [`httpd.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd.yml>`_]


.. _ug_apache_httpd_conf_ssl:

apache_httpd_conf_ssl
---------------------
.. highlight:: yaml
.. contents::
   :local:

Synopsis
^^^^^^^^
* ``apache_httpd_conf_ssl`` is a list of lines that configure SSL in httpd.conf.

Parameters
^^^^^^^^^^
============================= ==================== =============================
| *Parameter*                 | *Type*             | *Comments*
============================= ==================== =============================
| **line**                    | *string*           | The line to insert
                              | ``required``       | into the file
============================= ==================== =============================

Notes
^^^^^
.. note:: | * The default value is
	  |   ``apache_httpd_conf_ssl:``
          |     ``- "Include etc/apache{{ apache_version }}/extra/httpd-ssl.conf``
          | * For details see :ref:`as_ssl`. [`httpd-ssl.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-ssl.yml>`_]


.. _ug_apache_httpd_conf_ssl_extra:

apache_httpd_conf_ssl_extra
---------------------------
.. highlight:: yaml
.. contents::
   :local:

Synopsis
^^^^^^^^
* ``apache_httpd_conf_ssl_extra`` is a list of lines that configure SSL in extra/httpd-ssl.conf.

Parameters
^^^^^^^^^^
============================= ==================== =============================
| *Parameter*                 | *Type*             | *Comments*
============================= ==================== =============================
| **regexp**                  | *string*           | The pattern to replace if
                              | ``required``       | found
| **line**                    | *string*           | The line to insert/replace
                              | ``required``       | into the file
============================= ==================== =============================

Notes
^^^^^
.. note:: * See the default value in :ref:`ug_defaults`
          * For details see :ref:`as_ssl`. [`httpd-ssl.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-ssl.yml>`_]


.. _ug_apache_httpd_conf_ssl_extra_absent:

apache_httpd_conf_ssl_extra_absent
----------------------------------
.. highlight:: yaml
.. contents::
   :local:

Synopsis
^^^^^^^^
* ``apache_httpd_conf_ssl_extra_absent`` is a list of lines that will be removed from extra/httpd-ssl.conf.

Parameters
^^^^^^^^^^
============================= ==================== =============================
| *Parameter*                 | *Type*             | *Comments*
============================= ==================== =============================
| **regexp**                  | *string*           | The pattern to be removed
                              | ``required``       |
============================= ==================== =============================

Notes
^^^^^
.. note:: * The default value is empty list ``apache_httpd_conf_ssl_extra_absent: []``
          * For details see :ref:`as_ssl`. [`httpd-ssl.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-ssl.yml>`_]


.. _ug_apache_httpd_conf_ssl_listen:

apache_httpd_conf_ssl_listen
----------------------------
.. highlight:: yaml
.. contents::
   :local:

Synopsis
^^^^^^^^
* ``apache_httpd_conf_ssl_listen`` is a list of addresses and ports that the server will bind to.

Notes
^^^^^
.. note:: * | The default value is
	    | ``apache_httpd_conf_ssl_listen:``
	    |   ``- "Listen 443"``
	  * | Overlapping Listen directives will result in a fatal error which
	    | will prevent the server from starting up.
          * | For details see :ref:`as_ssl`. [`httpd-ssl.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-ssl.yml>`_]


.. _ug_apache_httpd_conf_modules:

apache_httpd_conf_modules
-------------------------
.. highlight:: yaml
.. contents::
   :local:

Synopsis
^^^^^^^^
* ``apache_httpd_conf_modules`` is a list of modules to be loaded.

Parameters
^^^^^^^^^^
============================= ==================== =============================
| *Parameter*                 | *Type*             | *Comments*
============================= ==================== =============================
| **module**                  | *string*           | Name of the module
                              | ``required``       |
| **mod**                     | *string*           | Object file or Library
                              | ``required``       |
| **present**                 | *boolean*          | If ``true`` LoadModule
                              | ``default: true``  | directive will be added to
                                                   | httpd.conf.
                                                   | If ``false`` directive will
                                                   | be commented (disabled).
============================= ==================== =============================

Example
^^^^^^^
.. code-block:: yaml
   :linenos:

   apache_httpd_conf_modules:
     - {module: "socache_shmcb_module", mod: "mod_socache_shmcb.so"}
     - {module: "ssl_module", mod: "mod_ssl.so"}
     - {module: "php5_module", mod: "libphp5.so"}

Notes
^^^^^
.. note:: * | The default value is
	    | ``apache_httpd_conf_modules:``
            |   ``- {module: "socache_shmcb_module", mod: "mod_socache_shmcb.so"}``
          * For details see :ref:`as_modules`. [`httpd-modules.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-modules.yml>`_]

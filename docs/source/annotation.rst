.. _as:

Annotated source code
*********************
.. contents:: Topics
   :depth: 3


.. _as_tree:

Tree
====
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


.. _as_variables:

Variables
=========

In this guide we describe role defaults variables in the directory ``defaults`` and
variables included from the directory ``vars``.

* role defaults in the directory ``{{ role_path }}/defaults``
  (precedence 2.)
* include OS specific vars from the directory ``{{ role_path }}/vars``
  (precedence 18.)

.. seealso:: `Variable precedence: Where should I put a variable?
             <https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable>`_


.. _as_os_vars:

Include OS specific variables
-----------------------------

Synopsis: Include OS specific variables from the role's directory vars.

OS specific default variables will be loaded from the files in the
directory `vars/defaults
<https://github.com/vbotka/ansible-apache/blob/master/vars/defaults/>`__. OS
specific custom variables, that will override default values, can be
loaded from the files in the directory `vars <https://github.com/vbotka/ansible-apache/blob/master/vars/>`_.

[`vars.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/vars.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/vars.yml
    :language: yaml
    :emphasize-lines: 10, 11
    :linenos:

.. seealso:: * See the precedence, naming conventions and other
               details in the included tasks (11) from
               `al_include_os_vars_path.yml
               <https://github.com/vbotka/ansible-lib/blob/master/tasks/al_include_os_vars_path.yml>`_.
	     * See :ref:`ug_variables`.


.. _as_packages:

Packages
========


.. _as_packages_install:

Install packages
----------------

Synopsis: Install packages for supported OS.

[`packages.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/packages.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/packages.yml
    :language: Yaml
    :emphasize-lines: 3,4
    :linenos:


.. _as_packages_install_freebsd:

Install FreeBSD packages
------------------------

[`packages-freebsd.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/packages-freebsd.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/packages-freebsd.yml
    :language: Yaml
    :emphasize-lines: 3,4,13,14
    :linenos:


.. _as_configure:

Configure
=========
.. contents:: Topics


.. _as_httpd-conf:

Configure httpd.conf
--------------------
.. highlight:: yaml
   :linenothreshold: 5
.. contents::
   :local:

Synopsis
^^^^^^^^
* Configure lines in httpd.conf

Annotation
^^^^^^^^^^
Iterate the list ``{{ apache_httpd_conf }}`` (9) and add lines to ``{{
apache_conf_path }}/httpd.conf`` (5).

Code
^^^^
[`httpd.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/httpd.yml
    :language: yaml
    :emphasize-lines: 5,9
    :linenos:

See Also
^^^^^^^^
.. seealso:: Variable :ref:`ug_apache_httpd_conf`


.. _as_includes:

Configure directories in Includes
---------------------------------
.. contents::
   :local:
   :depth: 3

Synopsis
^^^^^^^^
  * Create files with the directory blocks in the Includes directory.

Annotation
^^^^^^^^^^
Iterate the list ``{{ apache_directory_blocks }}`` (11) and create
configuration files in the directory ``{{ apache_conf_path
}}/Includes`` (6).

Code
^^^^
[`httpd-dirs.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-dirs.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/httpd-dirs.yml
    :language: yaml
    :emphasize-lines: 6,11
    :linenos:

.. _as_directory-block:

Template directory-block.j2
"""""""""""""""""""""""""""

[`directory-block.j2 <https://github.com/vbotka/ansible-apache/blob/master/templates/directory-block.j2>`_]

.. literalinclude:: ../../templates/directory-block.j2
    :language: jinja
    :emphasize-lines: 1,2
    :linenos:

See Also
^^^^^^^^

.. seealso:: Variable :ref:`ug_apache_directory_blocks`


.. _as_modules:

Configure modules
-----------------
.. contents::
   :local:
   :depth: 3


Synopsis
^^^^^^^^

  * Load Apache modules. Optionally configure PHP module.
  * (TODO: General configuration of modules.)


Annotation
^^^^^^^^^^

  * Iterate ``apache_httpd_conf_modules`` (11). When
    ``item.preset`` (4) insert line ``LoadModule ...`` (8) to
    httpd.conf (6)
  * Iterate ``apache_httpd_conf_modules`` (22). When ``not
    item.preset`` (15) comment line ``# LoadModule ...`` (20) in
    httpd.conf (17)
  * Configure PHP (30-38) in Includes/php.conf when ``apache_php`` (26)

Code
^^^^

[`httpd-modules.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-modules.yml>`_]

.. literalinclude:: ../../tasks/httpd-modules.yml
    :language: Yaml
    :emphasize-lines: 4,11,15,22,26,30-38
    :linenos:

See Also
^^^^^^^^

.. seealso:: Variable :ref:`ug_apache_httpd_conf_modules`


.. _as_alias:

Configure alias
---------------
.. contents::
   :local:
   :depth: 3

Synopsis
^^^^^^^^

  * Configure aliases in httpd.conf

Annotation
^^^^^^^^^^

* When not an empty list (4) iterate ``apache_alias`` (9) and include
  configuration lines in httpd/conf (6)


Code
^^^^

[`httpd-alias.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-alias.yml>`_]

.. literalinclude:: ../../tasks/httpd-alias.yml
    :language: Yaml
    :emphasize-lines: 4,6,9-11
    :linenos:

.. seealso:: Variable :ref:`ug_apache_alias`


.. _as_ssl:

Configure ssl
-------------
.. contents::
   :local:
   :depth: 3

Synopsis
^^^^^^^^
  * Configure SSL in extra/httpd-ssl.conf

Annotation
^^^^^^^^^^
  * Iterate ``apache_httpd_conf_ssl_extra`` (12) and configure lines
    in ``extra/httpd-ssl.conf``
  * Iterate ``apache_httpd_conf_ssl_extra_absent`` (20) and remove lines
    from ``extra/httpd-ssl.conf`` (17)
  * Iterate ``apache_httpd_conf_ssl_listen`` (27) and add lines
    from ``extra/httpd-ssl.conf`` (17)
  * Iterate ``apache_httpd_conf_ssl`` (34) and configure lines
    in ``httpd``

Code
^^^^

[`httpd-ssl.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-ssl.yml>`_]

.. literalinclude:: ../../tasks/httpd-ssl.yml
   :language: yaml
   :emphasize-lines: 4,8,12,17,20,24,27,31,34
   :linenos:

See Also
^^^^^^^^
.. seealso:: * Variable :ref:`ug_apache_httpd_conf_ssl`
	     * Variable :ref:`ug_apache_httpd_conf_ssl_extra`
	     * Variable :ref:`ug_apache_httpd_conf_ssl_extra_absent`
	     * Variable :ref:`ug_apache_httpd_conf_ssl_listen`


.. _as_vhosts:

Configure vhosts
----------------
.. highlight:: yaml
.. contents::
   :local:

Synopsis
^^^^^^^^

  * Configure virtual hosts in extra directory.

Annotation
^^^^^^^^^^

  * Loop the dictionary ``{{ apache_vhost }}`` (10,21,31) and

  * optionally (11) create directories ``DocumentRoot`` (5,6).

  * create files with the Apache virtual hosts in ``{{
    apache_conf_path }}/extra/`` (16). See :ref:`as_vhostj2` (15).

  * Include created files (28) in ``{{ apache_conf_path }}/httpd.conf`` (26).

Code
^^^^

[`httpd-vhosts.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-vhosts.yml>`_]

.. literalinclude:: ../../tasks/httpd-vhosts.yml
    :language: Yaml
    :emphasize-lines: 5-6,10-11,15-16,21,26,28,31
    :linenos:

.. _as_vhostj2:

Template vhost.j2
"""""""""""""""""

Create both http and https servers (1,8). Optionally ``default(True)``
redirect permanent http to https (4).

[`vhost.j2 <https://github.com/vbotka/ansible-apache/blob/master/templates/vhost.j2>`_]

.. literalinclude:: ../../templates/vhost.j2
   :language: jinja
   :emphasize-lines: 1,4,8
   :linenos:

See Also
^^^^^^^^
.. seealso::
   * Variable :ref:`ug_apache_vhost`


.. _as_confd:

Configure confd
---------------
.. contents::
   :local:
   :depth: 3

Synopsis
^^^^^^^^
* Configure virtual hosts

Annotation
^^^^^^^^^^
Configure virtual hosts in ``extra`` directory (4). Use
`encode_apache`_ filter. Configure sections in ``Includes`` directory
(8). Take the configuration data from the directories ``{{
apache_confd_dir_vhosts }}`` and ``{{ apache_confd_dir_sections
}}``. Include the files in ``httpd.conf``.

Code
^^^^

[`httpd-confd.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-confd.yml>`_]

.. literalinclude:: ../../tasks/httpd-confd.yml
    :language: Yaml
    :emphasize-lines: 4,8
    :linenos:


.. _as_confd-vhosts:

Configure confd-vhosts
----------------------
.. contents::
   :local:
   :depth: 3

Synopsis
^^^^^^^^
* Configure virtual hosts with the filter `encode_apache <https://galaxy.ansible.com/jtyr/config_encoder_filters>`_.
* Take the YAML configuration files from the directory ``{{
  apache_confd_dir_vhosts }}`` at master and create files with the
  Apache virtual hosts in ``{{ apache_conf_path }}/extra/`` at the
  remote host.
* The created files will be included in ``{{ apache_conf_path }}/httpd.conf``.
* The example of the YAML format of the configuration file is available at `vars/conf.d/vhosts-sample/example.com.yml <https://github.com/vbotka/ansible-apache/blob/master/vars/conf.d/vhosts-sample/example.com.yml>`_

Annotation
^^^^^^^^^^

Include data from conf.d (3-17)
"""""""""""""""""""""""""""""""

Include tasks from the file ``al_include_confd_vars_list`` (12) in the role
``vbotka.ansible_lib`` (11). This task takes as parameters the
directory with the YAML configuration files (7), and the type of the list
(8) and returns the list with the YAML configurations of the virtual
hosts stored in the variable ``al_include_confd_vars_list``. The
variable can be printed (17) if debug is turned on ``apache_debug:
true``. The parameters (7,8) are tested inside the included tasks.

.. seealso:: See details of the included tasks at
             `al_include_confd_vars_list.yml
             <https://github.com/vbotka/ansible-lib/blob/master/tasks/al_include_confd_vars_list.yml>`_.

Create directories for virtual hosts (25-27)
""""""""""""""""""""""""""""""""""""""""""""

Include tasks from fn/httpd-confd-vhost-dirs.yml .

.. seealso:: :ref:`as_confd-vhost-dirs`.

Configure virtual hosts in extra directory (29-38)
""""""""""""""""""""""""""""""""""""""""""""""""""

Create the Apache configuration files for the virtual hosts with the
help of ``encode_apache`` filter. Store the files in the directory
``{{ apache_conf_path }}/extra/``.

.. seealso:: For details see the template `vhost2.j2
             <https://github.com/vbotka/ansible-apache/blob/master/templates/vhost2.j2>`_.

Include virtual hosts in httpd.conf (40-48)
"""""""""""""""""""""""""""""""""""""""""""

Include virtual hosts in httpd.conf.

[`httpd-confd-vhosts.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-confd-vhosts.yml>`_]

.. literalinclude:: ../../tasks/httpd-confd-vhosts.yml
    :language: yaml
    :emphasize-lines: 3,7-8,11,12,17,25-27,29,32,37,40,44,47
    :linenos:

.. _as_confd-vhost-dirs:

Configure confd-vhost-dirs
""""""""""""""""""""""""""

* Create ``DocumentRoot`` directories for vhosts.

[`fn/httpd-confd-vhost-dirs.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/fn/httpd-confd-vhost-dirs.yml>`_]

.. literalinclude:: ../../tasks/fn/httpd-confd-vhost-dirs.yml
    :language: Yaml
    :emphasize-lines: 3
    :linenos:


.. _as_confd-includes:

Configure confd-includes
------------------------
.. contents::
   :local:
   :depth: 3

Synopsis
^^^^^^^^

* Configure sections with the filter encode_apache.

Annotation
^^^^^^^^^^
* Take the YAML configuration files from the directory ``{{ apache_confd_dir_sections }}`` at master and create files with the sections in ``{{ apache_conf_path }}/Includes/`` at the remote host. These created files are included in ``{{ apache_conf_path }}/httpd.conf`` by default.

.. code-block:: yaml

  $ grep Includes /usr/local/etc/apache24/httpd.conf
  Include etc/apache24/Includes/*.conf

* The example of the YAML format of the configuration file is
  available at `vars/conf.d/section-sample/usr-local-www-example.com.yml
  <https://github.com/vbotka/ansible-apache/blob/master/vars/conf.d/section-sample/usr-local-www-example.com.yml>`_
  and the details of the format are described in the filter
  `encode_apache
  <https://galaxy.ansible.com/jtyr/config_encoder_filters>`_.


Include data from conf.d (3-17)
"""""""""""""""""""""""""""""""

Include tasks from the file ``al_include_confd_vars_list`` (12) in the role
``vbotka.ansible_lib`` (11). This task takes as parameters the
directory with the YAML configuration files (7), and the type of the list
(8) and returns the list with the YAML configurations of the sections
stored in the variable ``al_include_confd_vars_list``. The variable
can be printed (17) if debug is turned on ``apache_debug: true``. The
parameters (7,8) are tested inside the included tasks.

.. seealso:: See details of the included tasks at
             `al_include_confd_vars_list.yml
             <https://github.com/vbotka/ansible-lib/blob/master/tasks/al_include_confd_vars_list.yml>`_.


Configure sections in Includes directory (19-28)
""""""""""""""""""""""""""""""""""""""""""""""""

Create the Apache configuration files for the sections with the
help of ``encode_apache`` filter. Store the files in the directory
``{{ apache_conf_path }}/Includes/`` (22).


[`httpd-confd-includes.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-confd-includes.yml>`_]

.. literalinclude:: ../../tasks/httpd-confd-includes.yml
    :language: yaml
    :emphasize-lines: 7,8,11,12,17,22
    :linenos:

.. _as_section2j2:

Template section2.j2
""""""""""""""""""""

[`section2.j2 <https://github.com/vbotka/ansible-apache/blob/master/templates/section2.j2>`_]

.. literalinclude:: ../../templates/section2.j2
   :language: jinja
   :emphasize-lines: 2
   :linenos:


.. _as_service:

Service
=======

[`service.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/service.yml>`_]

.. highlight:: yaml
   :linenothreshold: 5
.. literalinclude:: ../../tasks/service.yml
   :language: yaml
   :emphasize-lines: 5
   :linenos:

[`rcconf.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/rcconf.yml>`_]

.. highlight:: yaml
   :linenothreshold: 5
.. literalinclude:: ../../tasks/rcconf.yml
   :language: yaml
   :emphasize-lines: 4,8,13,17
   :linenos:


.. _as_handlers:

Handlers
========

[`main.yml <https://github.com/vbotka/ansible-apache/blob/master/handlers/main.yml>`_]

.. highlight:: yaml
   :linenothreshold: 5
.. literalinclude:: ../../handlers/main.yml
   :language: yaml
   :emphasize-lines: 4,10,16,22,28,32
   :linenos:

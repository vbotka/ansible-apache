.. _configure:

Configure
=========


.. _httpd-conf:

Configure httpd.conf
--------------------

.. highlight:: Yaml
    :linenothreshold: 5                                                                     

`httpd.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd.yml>`_

.. literalinclude:: ../../tasks/httpd.yml
    :language: Yaml
    :emphasize-lines: 9
    :linenos:


.. _includes:

Configure directories in Includes
---------------------------------

`httpd-dirs.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-dirs.yml>`_

.. literalinclude:: ../../tasks/httpd-dirs.yml
    :language: Yaml
    :emphasize-lines: 11
    :linenos:


.. _modules:

Configure modules
-----------------

`httpd-modules.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-modules.yml>`_

.. literalinclude:: ../../tasks/httpd-modules.yml
    :language: Yaml
    :emphasize-lines: 4,11,15,22,26,30-38
    :linenos:


.. _alias:

Configure alias
---------------

`httpd-alias.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-alias.yml>`_

.. literalinclude:: ../../tasks/httpd-alias.yml
    :language: Yaml
    :emphasize-lines: 4,7,9-11
    :linenos:


.. _ssl:

Configure ssl
-------------

`httpd-ssl.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-ssl.yml>`_

.. literalinclude:: ../../tasks/httpd-ssl.yml
    :language: Yaml
    :emphasize-lines: 4,8,12,17,20,24,27,31,34
    :linenos:


.. _vhosts:

Configure vhosts
----------------

Goal: Configure virtual hosts in extra directory.

Take the dictionary ``{{ apache_vhost }}`` and create files with the
Apache virtual hosts in ``{{ apache_conf_path }}/extra/``. The created
files will be included in ``{{ apache_conf_path }}/httpd.conf``.

See the example of the variable `vars/main.yml
<https://github.com/vbotka/ansible-apache/blob/master/vars/main.yml>`_.

`httpd-vhosts.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-vhosts.yml>`_

.. literalinclude:: ../../tasks/httpd-vhosts.yml
    :language: Yaml
    :emphasize-lines: 4-6,10-11
    :linenos:


.. _confd:

Configure confd
---------------

Goal: Configure virtual hosts in ``extra`` directory (4). Use
`encode_apache`_ filter. Configure sections in ``Includes`` directory
(8).

Take the configuration data from the directories ``{{
apache_confd_dir_vhosts }}`` and ``{{ apache_confd_dir_sections
}}``. Include the files in ``httpd.conf``.

`httpd-confd.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-confd.yml>`_

.. literalinclude:: ../../tasks/httpd-confd.yml
    :language: Yaml
    :emphasize-lines: 4,8
    :linenos:


.. _confd-vhosts:

Configure confd-vhosts
----------------------

Goal: Configure virtual hosts with the filter encode_apache.

Take the YAML configuration files from the directory ``{{
apache_confd_dir_vhosts }}`` at master and create files with the
Apache virtual hosts in ``{{ apache_conf_path }}/extra/`` at the
remote host. The created files will be included in ``{{
apache_conf_path }}/httpd.conf``.

The example of the YAML format of the configuration file is available
at `vars/conf.d/vhosts-sample/example.com.yml
<https://github.com/vbotka/ansible-apache/blob/master/vars/conf.d/vhosts-sample/example.com.yml>`_
and the details of the format are described in the filter
`encode_apache
<https://galaxy.ansible.com/jtyr/config_encoder_filters>`_.


Include data from conf.d (3-17)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Include tasks from the file ``al_include_confd_vars_list`` (12) in the role
``vbotka.ansible_lib`` (11). This task takes as parameters the
directory with the YAML configuration files (7), and the type of the list
(8) and returns the list with the YAML configurations of the virtual
hosts stored in the variable ``al_include_confd_vars_list``. The
variable can be printed (17) if debug is turned on ``apache_debug:
true``. The parameters (7,8) are tested inside the included tasks. See
details of the included tasks at `al_include_confd_vars_list.yml
<https://github.com/vbotka/ansible-lib/blob/master/tasks/al_include_confd_vars_list.yml>`_.


Create directories for virtual hosts (25-27)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Include tasks from fn/httpd-confd-vhost-dirs.yml . See :ref:`Configure
confd-vhost-dirs <confd-vhost-dirs>`


Configure virtual hosts in extra directory (29-38)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Create the Apache configuration files for the virtual hosts with the
help of ``encode_apache`` filter. Store the files in the directory
``{{ apache_conf_path }}/extra/`` For details see the template
`vhost2.j2
<https://github.com/vbotka/ansible-apache/blob/master/templates/vhost2.j2>`_.


Include virtual hosts in httpd.conf (40-48)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Include virtual hosts in httpd.conf.


`httpd-confd-vhosts.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-confd-vhosts.yml>`_

.. literalinclude:: ../../tasks/httpd-confd-vhosts.yml
    :language: Yaml
    :emphasize-lines: 3,7-8,11,12,17,25-27,29,32,37,40,44,47
    :linenos:


.. _confd-vhost-dirs:

Configure confd-vhost-dirs
--------------------------

Goal: Create ``DocumentRoot`` directories for vhosts.

`fn/httpd-confd-vhost-dirs.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/fn/httpd-confd-vhost-dirs.yml>`_

.. literalinclude:: ../../tasks/fn/httpd-confd-vhost-dirs.yml
    :language: Yaml
    :emphasize-lines: 3
    :linenos:


.. _confd-includes:

Configure confd-includes
------------------------

Goal: Configure sections with the filter encode_apache.

Take the YAML configuration files from the directory ``{{
apache_confd_dir_sections }}`` at master and create files with the
sections in ``{{ apache_conf_path }}/Includes/`` at the
remote host. These created files are included in ``{{
apache_conf_path }}/httpd.conf`` by default. See below
::

  $ grep Includes /usr/local/etc/apache24/httpd.conf
  Include etc/apache24/Includes/*.conf

The example of the YAML format of the configuration file is available
at `vars/conf.d/section-sample/usr-local-www-example.com.yml <https://github.com/vbotka/ansible-apache/blob/master/vars/conf.d/section-sample/usr-local-www-example.com.yml>`_
and the details of the format are described in the filter
`encode_apache
<https://galaxy.ansible.com/jtyr/config_encoder_filters>`_.


Include data from conf.d (3-17)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Include tasks from the file ``al_include_confd_vars_list`` (12) in the role
``vbotka.ansible_lib`` (11). This task takes as parameters the
directory with the YAML configuration files (7), and the type of the list
(8) and returns the list with the YAML configurations of the sections
stored in the variable ``al_include_confd_vars_list``. The variable
can be printed (17) if debug is turned on ``apache_debug: true``. The
parameters (7,8) are tested inside the included tasks. See details of
the included tasks at `al_include_confd_vars_list.yml
<https://github.com/vbotka/ansible-lib/blob/master/tasks/al_include_confd_vars_list.yml>`_.

Configure sections in Includes directory (19-28)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Create the Apache configuration files for the sections with the
help of ``encode_apache`` filter. Store the files in the directory
``{{ apache_conf_path }}/Includes/`` (22). For details see the template
`section2.j2
<https://github.com/vbotka/ansible-apache/blob/master/templates/sectiont2.j2>`_.


`httpd-confd-includes.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/httpd-confd-includes.yml>`_

.. literalinclude:: ../../tasks/httpd-confd-includes.yml
    :language: Yaml
    :emphasize-lines: 7,8,11,12,17,22
    :linenos:

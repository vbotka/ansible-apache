Tasks
=====

.. _as_main.yml:

main.yml
--------

Synopsis: Main task.


Import tasks if enabled.


[`tasks/main.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/main.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/main.yml
    :language: Yaml
    :emphasize-lines: 1,2
    :linenos:





.. _as_vars.yml:

vars.yml
--------

Synopsis: Include OS specific variables from the role's directory vars.



OS specific default variables will be loaded from the files in the directories `vars
<https://github.com/vbotka/ansible-apache/blob/master/vars/>`_ and `vars/defaults
<https://github.com/vbotka/ansible-apache/blob/master/vars/defaults/>`_. OS specific custom
variables, that will override default values, can be loaded from the files in the directory `vars
<https://github.com/vbotka/ansible-apache/blob/master/vars/>`_.


[`tasks/vars.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/vars.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/vars.yml
    :language: Yaml
    :emphasize-lines: 10,11
    :linenos:

.. seealso::
   * See the precedence, naming conventions and other details in the included task (11) from `al_include_os_vars_path.yml <https://raw.githubusercontent.com/vbotka/ansible-lib/devel/tasks/al_include_os_vars_path.yml>`_
   * See :ref:`ug_variables`

.. note::
   * Put OS specific variables here only. Because of the precedence (15.role vars), there are limited options to override these variables, if necessary.

.. hint::
   * It might be more convenient to maintain the variables incrementally. See `al_include_os_vars_path_incr.yml <https://raw.githubusercontent.com/vbotka/ansible-lib/devel/tasks/al_include_os_vars_path_incr.yml>`_

.. warning::
   * Put customized OS specific variables into the files in the dictionary *vars/*. Changes stored in the directory *vars/defaults* will be overwritten by an update of the role.

.. _as_debug.yml:

debug.yml
---------

Synopsis: Configure debug.


Description of the task.


[`tasks/debug.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/debug.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/debug.yml
    :language: Yaml
    :emphasize-lines: 1,2
    :linenos:





.. _as_packages.yml:

packages.yml
------------

Synopsis: 
Install packages for supported OS.



<TBD>


[`tasks/packages.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/packages.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/packages.yml
    :language: Yaml
    :emphasize-lines: 3
    :linenos:

.. seealso::
   * <TBD>




.. _as_packages-freebsd.yml:

packages-freebsd.yml
--------------------

Synopsis: 
<TBD>



<TBD>


[`tasks/packages-freebsd.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/packages-freebsd.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/packages-freebsd.yml
    :language: Yaml
    :emphasize-lines: 3,13
    :linenos:

.. seealso::
   * <TBD>




.. _as_samples.yml:

samples.yml
-----------

Synopsis: 
<TBD>



<TBD>


[`tasks/samples.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/samples.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/samples.yml
    :language: Yaml
    :emphasize-lines: 3
    :linenos:

.. seealso::
   * <TBD>




.. _as_httpd.yml:

httpd.yml
---------

Synopsis: Configure lines in httpd.conf



Iterate the list ``apache_httpd_conf`` (9) and add lines to the configuration file (5).


[`tasks/httpd.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/httpd.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/httpd.yml
    :language: Yaml
    :emphasize-lines: 5,9
    :linenos:

.. seealso::
   * Variable :ref:`ug_apache_httpd_conf`




.. _as_httpd-dirs.yml:

httpd-dirs.yml
--------------

Synopsis: 
Create files with the directory blocks in the Includes directory.



Iterate the list ``apache_directory_blocks`` (11) and create configuration files in the directory (6).


[`tasks/httpd-dirs.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/httpd-dirs.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/httpd-dirs.yml
    :language: Yaml
    :emphasize-lines: 6,11
    :linenos:

.. seealso::
   * Template :ref:`as_template_directory-block.j2`
   * Variable :ref:`ug_apache_directory_blocks`




.. _as_httpd-modules.yml:

httpd-modules.yml
-----------------

Synopsis: 
Load Apache modules. Optionally configure PHP module. (TODO: General configuration of modules.)



Iterate ``apache_httpd_conf_modules`` (11). When ``item.preset`` (4) insert line ``LoadModule
...`` (8) to httpd.conf (6). Iterate ``apache_httpd_conf_modules`` (22). When ``not item.preset``
(15) comment line ``# LoadModule ...`` (20) in httpd.conf (17). Configure PHP (30-38) in
Includes/php.conf when ``apache_php`` (26).


[`tasks/httpd-modules.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/httpd-modules.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/httpd-modules.yml
    :language: Yaml
    :emphasize-lines: 4,11,15,22,26,30-38
    :linenos:

.. seealso::
   * Variable :ref:`ug_apache_httpd_conf_modules`




.. _as_httpd-alias.yml:

httpd-alias.yml
---------------

Synopsis: 
Configure aliases in httpd.conf



When not an empty list (4) iterate ``apache_alias`` (9) and include configuration lines in httpd/conf (6).


[`tasks/httpd-alias.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/httpd-alias.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/httpd-alias.yml
    :language: Yaml
    :emphasize-lines: 4,6,9-11
    :linenos:

.. seealso::
   * Variable :ref:`ug_apache_alias`




.. _as_httpd-ssl.yml:

httpd-ssl.yml
-------------

Synopsis: 
Configure SSL in extra/httpd-ssl.conf



Iterate ``apache_httpd_conf_ssl_extra`` (12) and configure lines in
``extra/httpd-ssl.conf``. Iterate ``apache_httpd_conf_ssl_extra_absent`` (20) and remove lines
from ``extra/httpd-ssl.conf`` (17). Iterate ``apache_httpd_conf_ssl_listen`` (27) and add lines
from ``extra/httpd-ssl.conf`` (17). Iterate ``apache_httpd_conf_ssl`` (34) and configure lines in
``httpd.conf``.


[`tasks/httpd-ssl.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/httpd-ssl.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/httpd-ssl.yml
    :language: Yaml
    :emphasize-lines: 4,8,12,17,20,24,27,31,34
    :linenos:

.. seealso::
   * Variable :ref:`ug_apache_httpd_conf_ssl`
   * Variable :ref:`ug_apache_httpd_conf_ssl_extra`
   * Variable :ref:`ug_apache_httpd_conf_ssl_extra_absent`
   * Variable :ref:`ug_apache_httpd_conf_ssl_listen`




.. _as_httpd-vhosts.yml:

httpd-vhosts.yml
----------------

Synopsis: 
Configure virtual hosts in extra directory.



Loop the dictionary ``apache_vhost`` (10,21,31) and optionally (11) create directories
``DocumentRoot`` (5,6). Create files with the Apache virtual hosts in the directory (16). See the
template :ref:`as_template_vhost.j2` (15). Include created files (28) in the configuration file
(26).


[`tasks/httpd-vhosts.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/httpd-vhosts.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/httpd-vhosts.yml
    :language: Yaml
    :emphasize-lines: 5-6,10-11,15-16,21,26,28,31
    :linenos:

.. seealso::
   * Template :ref:`as_template_vhost.j2`
   * Template :ref:`as_template_vhost2.j2`




.. _as_httpd-confd.yml:

httpd-confd.yml
---------------

Synopsis: 
Configure virtual hosts.



Configure virtual hosts (4) and configuration sections of the directories (9).


[`tasks/httpd-confd.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/httpd-confd.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/httpd-confd.yml
    :language: Yaml
    :emphasize-lines: 4,9
    :linenos:

.. seealso::
   * httpd-confd-vhosts.yml and httpd-confd-includes.yml




.. _as_httpd-confd-vhosts.yml:

httpd-confd-vhosts.yml
----------------------

Synopsis: 
Configure virtual hosts. Create files.



Configure virtual hosts with the filter `encode_apache
<https://galaxy.ansible.com/jtyr/config_encoder_filters>`_. See the template vhost2.j2.  Take the
YAML configuration files from the directory ``apache_confd_dir_vhosts`` (7) at master and create
files with the Apache virtual hosts in the directory (32) at the remote host. The created files
will be included in the configuration file (42).


**Include data from conf.d (3-17)**

Include tasks from the file ``al_include_confd_vars_list`` (12) in the role ``vbotka.ansible_lib``
(11). This task takes as parameters the directory with the YAML configuration files (7), and the
type of the list (8) and returns the list with the YAML configurations of the virtual hosts stored
in the variable ``al_include_confd_vars_list``. The variable can be printed (17) if debug is
turned on ``apache_debug: true``. The parameters (7,8) are tested inside the included tasks.


**Create directories for virtual hosts (25-27)**

Include tasks from ``fn/httpd-confd-vhost-dirs.yml`` .


**Configure virtual hosts in extra directory (29-38)**

Create the Apache configuration files for the virtual hosts with the help of ``encode_apache``
filter. Store the configuration file (32).


**Include virtual hosts in httpd.conf (40-48)**

Include virtual hosts in httpd.conf.


[`tasks/httpd-confd-vhosts.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/httpd-confd-vhosts.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/httpd-confd-vhosts.yml
    :language: Yaml
    :emphasize-lines: 3,7-8,11,12,17,25-27,29,32,37,40,42,44,47
    :linenos:

.. seealso::
   * Template :ref:`as_template_vhost2.j2`
   * Example of the configuration file `vars/conf.d/vhosts-sample/example.com.yml <https://github.com/vbotka/ansible-apache/blob/master/vars/conf.d/vhosts-sample/example.com.yml>`_
   * Included task `al_include_confd_vars_list.yml <https://github.com/vbotka/ansible-lib/blob/master/tasks/al_include_confd_vars_list.yml>`_.




.. _as_httpd-confd-vhost-dirs.yml:

httpd-confd-vhost-dirs.yml
--------------------------

Synopsis: 
Create ``DocumentRoot`` directories for vhosts.



<TBD>


[`tasks/fn/httpd-confd-vhost-dirs.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/fn/httpd-confd-vhost-dirs.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/fn/httpd-confd-vhost-dirs.yml
    :language: Yaml
    :emphasize-lines: 2
    :linenos:

.. seealso::
   * <TBD>




.. _as_httpd-confd-includes.yml:

httpd-confd-includes.yml
------------------------

Synopsis: 
Configure sections with the filter encode_apache.



Take the YAML configuration files from the directory ``apache_confd_dir_sections`` (7) at master
and create the configuration files (22) at the remote host. The created configuration files are
included in the configuration file ``httpd.conf`` by default.

.. code-block:: yaml

  shell> grep Includes /usr/local/etc/apache24/httpd.conf
  Include etc/apache24/Includes/*.conf

**Include data from conf.d (3-17)**

Include tasks from the file ``al_include_confd_vars_list`` (12) in the role ``vbotka.ansible_lib``
(11). This task takes as parameters the directory with the YAML configuration files (7), and the
type of the list (8) and returns the list with the YAML configurations of the sections stored in
the variable ``al_include_confd_vars_list``. The parameters (7,8) are tested inside the included
tasks.

**Configure sections in Includes directory (19-28)**

Create the Apache configuration files (22) for the sections with the help of ``encode_apache``
filter.


[`tasks/httpd-confd-includes.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/httpd-confd-includes.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/httpd-confd-includes.yml
    :language: Yaml
    :emphasize-lines: 7,8,11,12,17,22
    :linenos:

.. seealso::
   * Template :ref:`as_template_section2.j2`
   * Example of the configuration file `vars/conf.d/section-sample/usr-local-www-example.com.yml <https://github.com/vbotka/ansible-apache/blob/master/vars/conf.d/section-sample/usr-local-www-example.com.yml>`_
   * Details of the format are described in the filter `encode_apache <https://galaxy.ansible.com/jtyr/config_encoder_filters>`_




.. _as_service.yml:

service.yml
-----------

Synopsis: 
Configure service.



At the moment, only configuration of FreeBSD is implemented (3).


[`tasks/service.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/service.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/service.yml
    :language: Yaml
    :emphasize-lines: 3
    :linenos:

.. seealso::
   * rcconf.yml




.. _as_rcconf.yml:

rcconf.yml
----------

Synopsis: 
Configure service in FreeBSD.



Enable the service (3) or disable it (12).


[`tasks/rcconf.yml <https://github.com/vbotka/ansible-freebsd-postinstall/blob/2.0-stable/tasks/rcconf.yml>`_]

.. highlight:: yaml
    :linenothreshold: 5
.. literalinclude:: ../../tasks/rcconf.yml
    :language: Yaml
    :emphasize-lines: 3,12
    :linenos:

.. seealso::
   * <TBD>





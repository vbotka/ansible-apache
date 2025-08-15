===============================
vbotka.apache 2.7 Release Notes
===============================

.. contents:: Topics


2.7.2
=====

Release Summary
---------------
By default, if apache_confd=false (default=true), configure httpd.conf
and Includes only.

Major Changes
-------------

Minor Changes
-------------
* Add dictionary apache_ansible_lib; Import vbotka.freebsd.lib in
  collection vbotka.freebsd
* Change default apache_httpd_conf_modules=[]
* Add var apache_confd (default=true)
* Update tasks debug.yml
* Update default al_include_confd_vars_list=[]
* Add conditions apache_php, apache_ssl, and apache_confd to main.yml


2.7.1
=====

Release Summary
---------------
Include this role in the collection vbotka.freebsd

Major Changes
-------------

Minor Changes
-------------
* Add ansible_role_name to debug.yml
* Use community.general.sysrc to configure rc.conf
* Use vbotka.freebsd.lib in the collection vbotka.freebsd
* Handlers use vbotka.freebsd.service in the collection vbotka.freebsd
* Update tasks names and formatting.
* Update docs.


2.7.0
=====

Release Summary
---------------
Ansible 2.18 update

Major Changes
-------------
* Supported FreeBSD 13.4, 13.5, 14.1, 14.2, 14.3

Minor Changes
-------------
* Updated documentation. Updated annotation templates
* Added .gitignore

Bugfixes
--------

Breaking Changes / Porting Guide
--------------------------------

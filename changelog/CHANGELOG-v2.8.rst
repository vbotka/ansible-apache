===============================
vbotka.apache 2.8 Release Notes
===============================

.. contents:: Topics


2.8.2
=====

Release Summary
---------------
Maintenance update.

Major Changes
-------------

Minor Changes
-------------
Fix docs annotation/vars/httpd-confd-vhosts.yml
Fix handlers notification.


2.8.1
=====

Release Summary
---------------
Ansible 2.20 upgrade.

Major Changes
-------------

Minor Changes
-------------
* Meta: Ansible 2.20; FreeBSD 13.5, 14.3, and 15.0
* Variables ansible_* moved to the dictionary ansible_facts


2.8.0
=====

Release Summary
---------------

Major Changes
-------------
* Meta: Ansible 2.19; FreeBSD 13.5, 14.2, and 14.3

Minor Changes
-------------

* Update handlers for vbotka.apache and vbotka.freebsd.apache; Use module
  vbotka.freebsd.service in the collection.

Bugfixes
--------

Breaking Changes / Porting Guide
--------------------------------

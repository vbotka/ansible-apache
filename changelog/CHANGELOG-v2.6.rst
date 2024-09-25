===============================
vbotka.apache 2.6 Release Notes
===============================

.. contents:: Topics
# BEGIN Release notes 2.6.6
2.6.6
=====
Release Summary
---------------
Major Changes
-------------
Minor Changes
-------------
- Update python 3.11 in .travis.yml
- Format meta/main.yml
- Update README.
- Update README
- Start 2.6.6 devel.

Bugfixes
--------
Breaking Changes / Porting Guide
--------------------------------
# END Release notes 2.6.6


2.6.6
=====

Release Summary
---------------
Maintenance including docs update.

Major Changes
-------------

Minor Changes
-------------
- Update README
- Update python 3.11 in .travis.yml
- Format meta/main.yml
- Update README

Bugfixes
--------

Breaking Changes / Porting Guide
--------------------------------


2.6.5
=====

Release Summary
---------------
Maintenance and features including docs update.

Major Changes
-------------
* Add tasks sanity.yml, httpd-php.yml
* Add var apache_sanity default=true
* Add var apache_php_fpm default=false
* Add vars apache_php_include and apache_php_fpm_include

Minor Changes
-------------
* Update debug.yml, httpd-modules.yml
* Split defaults/main.yml to defaults/main/*
* Update docs

Bugfixes
--------

Breaking Changes / Porting Guide
--------------------------------


2.6.4
=====

Release Summary
---------------
Maintenance including docs update.

Major Changes
-------------
* Upgrade default apache_php_version: "83"
* If apache_php add package apache_php_package to apache_packages

Minor Changes
-------------
* Bump docs and defaults to 2.6.4
* Update README


2.6.3
=====

Release Summary
---------------
Ansible 2.17 maintenance and bugfix update with updated docs.

Major Changes
-------------
* Add support of 14.1

Minor Changes
-------------
* Bump docs 2.6.3
* Remove obsolete comment from docs/source/conf.py
* Update README
* Add var apache_role_version
* Add var apache_sslciphersuite_list


2.6.2
=====

Release Summary
---------------
Bugfix release with updated docs.

Major Changes
-------------

Minor Changes
-------------
* Bump docs version.
* Update docs.
* Fix Ansible lint empty-lines
* Use default rules in local ansible-lint config.
* Update skip_list in local ansible-lint config.


2.6.1
=====

Release Summary
---------------
Bugfix release with updated docs.

Major Changes
-------------

Minor Changes
-------------
* Bump docs version.
* Fix docs formatting.
* Fix docs links.
* Fix handler notifications.


2.6.0
=====

Release Summary
---------------
Ansible 2.16 update

Major Changes
-------------
* Support FreeBSD 13.3. and 14.0

Minor Changes
-------------
* Bump docs version 2.6.0
* Update docs.
* Update ansible lint config.
* Update requirements.yml
* Update README.
* Formatting travis.yml
* Fix Ansible lint.

Bugfixes
--------

Breaking Changes / Porting Guide
--------------------------------

.. _variables:

Variables
=========

.. _vars_default:

Include OS specific variables
-----------------------------

Goal: Include OS specific variables from the role's directory vars.

OS specific default variables shall be loaded from the files in the
directory `vars/defaults
<https://github.com/vbotka/ansible-apache/blob/master/vars/defaults/>`__. OS
specific custom variables, that will override default values, can be
loaded from the files in the directory `vars <https://github.com/vbotka/ansible-apache/blob/master/vars/>`__.

See the precedence, naming conventions and other details in the included tasks (11) at `al_include_os_vars_path.yml
<https://github.com/vbotka/ansible-lib/blob/master/tasks/al_include_os_vars_path.yml>`_.

.. highlight:: Yaml
    :linenothreshold: 5                                                                     

`vars.yml <https://github.com/vbotka/ansible-apache/blob/master/tasks/vars.yml>`_

.. literalinclude:: ../../tasks/vars.yml
    :language: Yaml
    :emphasize-lines: 10, 11
    :linenos:


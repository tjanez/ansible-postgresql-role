PostgreSQL
==========

Ansible role to install and configure the PostgreSQL database on RHEL/CentOS 7
systems using the PostgreSQL `Red Hat/CentOS Software Collection`_.

**NOTE: This version of the role only supports installing PostgreSQL 10.**

**Main features:**

- Installs PostgreSQL from `Red Hat/CentOS Software Collection`_.
- Upgrades PostgreSQL from a previous version if it was installed as a
  Software Collection. For supported versions to upgrade from, see the
  ``postgresql_upgrade_ids`` list in ``vars/main.yml``.
- Can install PostgreSQL Software Collection as the system-installed PostgreSQL
  using the `syspaths package`_.

.. _Red Hat/CentOS Software Collection:
  https://developers.redhat.com/products/softwarecollections/overview/
.. _syspaths package:
  https://developers.redhat.com/blog/2017/10/18/use-software-collections-without-bothering-alternative-path/


Requirements
------------

This role requires Ansible 2.4 or higher.


Role Variables
--------------

Variables in ``defaults/main.yml``:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

+-----------------------------------------+----------+--------------------------------------------+
|                Name                     |   Type   |                Description                 |
+=========================================+==========+============================================+
| ``postgresql_conf_listen_addresses``    | string   | Comma-separeted list of IP addresses to    |
|                                         |          | listen on.                                 |
|                                         |          |                                            |
|                                         |          | *NOTE*: Use ``"*"`` to specify to listen   |
|                                         |          | on all addresses.                          |
+-----------------------------------------+----------+--------------------------------------------+
| ``postgresql_conf_password_encryption`` | string   | Algorithm to use when encrypting           |
|                                         |          | passwords.                                 |
|                                         |          |                                            |
|                                         |          | Available options are:                     |
|                                         |          |                                            |
|                                         |          | - ``scram-sha-256``                        |
|                                         |          | - ``md5``                                  |
+-----------------------------------------+----------+--------------------------------------------+
| ``postgresql_conf_hba_local``           | dict     | Dictionary specifying authentication       |
|                                         |          | methods for local connections in           |
|                                         |          | ``pg_hba.conf`` file.                      |
|                                         |          |                                            |
|                                         |          | It must contain three keys controlling     |
|                                         |          | different types of local connections:      |
|                                         |          |                                            |
|                                         |          | - ``unix_socket``: local Unix socket       |
|                                         |          |   connections                              |
|                                         |          | - ``ipv4``: local IPv4 connections         |
|                                         |          | - ``ipv6``: local IPv6 connections         |
|                                         |          |                                            |
|                                         |          | Each key's value must specify a valid      |
|                                         |          | authentication method for its connection   |
|                                         |          | type.                                      |
|                                         |          |                                            |
|                                         |          | For a list of possible authentication      |
|                                         |          | methods, read the `documentation on        |
|                                         |          | pg_hba.conf file`_.                        |
+-----------------------------------------+----------+--------------------------------------------+
| ``postgresql_conf_hba_custom``          | list     | List specifying custom records in the      |
|                                         |          | ``pg_hba.conf`` file.                      |
|                                         |          |                                            |
|                                         |          | Each entry is a dictionary of the form:    |
|                                         |          |                                            |
|                                         |          | .. code-block:: yaml                       |
|                                         |          |                                            |
|                                         |          |     description: string                    |
|                                         |          |     conn_type: string                      |
|                                         |          |     database: string                       |
|                                         |          |     user: string                           |
|                                         |          |     address: string                        |
|                                         |          |     method: string                         |
|                                         |          |                                            |
|                                         |          | where ``description`` represents record's  |
|                                         |          | description, ``conn_type``, ``database``,  |
|                                         |          | ``user``, ``address`` and ``method``       |
|                                         |          | represent ``pg_hba.conf``'s connection     |
|                                         |          | type, database, user, address and          |
|                                         |          | authentication method parameters.          |
|                                         |          |                                            |
|                                         |          | For a detailed description of each         |
|                                         |          | configuration parameter, read the          |
|                                         |          | `documentation on pg_hba.conf file`_.      |
+-----------------------------------------+----------+--------------------------------------------+
| ``postgresql_install_syspaths``         | boolean  | Specifies whether to install PostgreSQL    |
|                                         |          | Software Collection as the                 |
|                                         |          | system-installed PostgreSQL using the      |
|                                         |          | `syspaths package`_.                       |
+-----------------------------------------+----------+--------------------------------------------+
| ``postgresql_rhel_rhscl_repo``          | boolean  | Name of the Red Hat Software Collections   |
|                                         |          | repository.                                |
+-----------------------------------------+----------+--------------------------------------------+

.. _documentation on pg_hba.conf file:
  https://www.postgresql.org/docs/current/static/auth-pg-hba-conf.html

Variables in ``vars/main.yml``:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

+-----------------------------------------+----------+--------------------------------------------+
|                Name                     |   Type   |                Description                 |
+=========================================+==========+============================================+
| ``postgresql_major_version``            | string   | Major PostgreSQL version of the current    |
|                                         |          | PostgreSQL Software Collection.            |
+-----------------------------------------+----------+--------------------------------------------+
| ``postgresql_scl_name``                 | string   | Name of the current PostgreSQL Software    |
|                                         |          | Collection.                                |
+-----------------------------------------+----------+--------------------------------------------+
| ``postgresql_package_name``             | string   | Name of the main RPM package.              |
+-----------------------------------------+----------+--------------------------------------------+
| ``postgresql_unit_name``                | string   | Name of the systemd unit.                  |
+-----------------------------------------+----------+--------------------------------------------+
| ``postgresql_data_dir``                 | string   | Path to PostgreSQL's data directory.       |
+-----------------------------------------+----------+--------------------------------------------+
| ``postgresql_bin_dir``                  | string   | Path to PostgreSQL Software Collection's   |
|                                         |          | binaries directory.                        |
+-----------------------------------------+----------+--------------------------------------------+
| ``postgresql_setup_command``            | boolean  | Path to PostgreSQL's setup command.        |
+-----------------------------------------+----------+--------------------------------------------+
| ``postgresql_scl_enable_command``       | boolean  | Software Collection's ``scl enable``       |
|                                         |          | command.                                   |
+-----------------------------------------+----------+--------------------------------------------+
| ``postgresql_upgrade_ids``              | list     | List of previous PostgreSQL Software       |
|                                         |          | Collections from which it is possible to   |
|                                         |          | upgrade to the current PostgreSQL Software |
|                                         |          | Collection.                                |
+-----------------------------------------+----------+--------------------------------------------+


Dependencies
------------

None.


Example Playbook
----------------

.. code-block:: yaml

    - hosts: all

      roles:
        - tjanez.postgresql


License
-------

GPLv3


Author Information
------------------

Tadej Janež


Acknowledgement
---------------

This Ansible role was originally developed for `Genialis`_. With approval from
Genialis, the code was generalised and published as Open Source, for which the
author would like to express his gratitude.

.. _Genialis:
  https://www.genialis.com/

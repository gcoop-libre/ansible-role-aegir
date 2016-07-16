Aegir
=====

The Role installs Aegir, a control panel for deploying and managing large networks of Drupal sites.

Requirements
------------

A MySQL server is required.

This server can be installed on the same machine, or a separate one (hence why this isn't listed as a dependency).

See the 'Example Playbook' section below for a simple method of installing a MySQL server with Ansible.

If Aegir's MySQL user and password are not defined, then the `mysql_root_username` and `mysql_root_password` are used.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    aegir_root: /var/aegir

The directory where Aegir will be installed.

    aegir_user: aegir

System User wich will execute Aegir's drush commands.

    aegir_web_group: www-data

System Group of the Web Server process (Apache / Nginx)

    aegir_db_host: localhost
    aegir_db_user: "{{ mysql_root_username | default('root') }}"
    aegir_db_pass: "{{ mysql_root_password | default('root') }}"

Database information for Aegir.

    aegir_manage_dependencies: true
    aegir_dependencies:
      - mysql-client
      - libapache2-mod-php5
      - php5
      - php5-cli
      - php5-gd
      - php5-mysql
      - postfix
      - sudo
      - rsync
      - git
      - unzip

This properties enables the management of the Aegir dependencies and establishes the system packages that will be installed.

    aegir_install_additional_packages: true
    aegir_additional_packages:
      - php5-curl

This properties enables the installation of aditional packages for Aegir and establishes the system packages that will be installed.

    aegir_provision_repo: http://git.drupal.org/project/provision.git
    aegir_provision_version: 7.x-3.x
    aegir_provision_update: false

Repository and vesion of the Aegir provision to be installed.

    aegir_http_service_type: apache

HTTP server that will be used for Aegir.

    aegir_makefile: aegir.make
    aegir_makefile_contents: "{{ lookup('file', aegir_makefile) }}"

Make file's name for Aegir's deploy and it contents. By default, it reads the content of the file defined in `aegir_makefile`.

    aegir_platform_version: 7.x-3.x
    aegir_frontend_url: aegir.local

This properties establish the version of the Aegir platform to be installed, and the URL for it's web interface.

    aegir_generate_keypair: true

This property defines if the SSH Keypair for Aegir's system user will be generated during this installation.

Dependencies
------------

  - gcoop-libre.drush (Installs Drush). Note that this role currently defaults to the Drush 7.x branch. As a result, Drupal 8 isn't supported by default.

Example Playbook
----------------

    - hosts: servers
      roles:
        - geerlingguy.mysql
        - gcoop-libre.aegir

After the playbook runs, the Aegir front-end site will be available, as will the Drush extensions (Provision, et. al.) that do the heavy lifting.

License
-------

MIT / BSD

Author Information
------------------

This role was modified by [gcoop Cooperativa de Software Libre](http://gcoop.coop) in 2016.

It was originally created in 2015 by [Christopher Gervais](http://ergonlogic.com/), lead maintainer of the [Aegir Hosting System](http://www.aegirproject.org).

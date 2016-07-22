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

    aegir_settings_mail_disable: false

Disable mail sending using the Devel module.

    aegir_settings_syslog: true
    aegir_settings_syslog_facility: 128
    aegir_settings_syslog_format: '!base_url|!timestamp|!type|!ip|!request_uri|!referer|!uid|!link|!message'
    aegir_settings_syslog_prefix: drupal_

Enable syslog configuration and set some basic configs.

    aegir_settings_extras: []

Here you can specify other vars for the settings.php files. Each config must be a dictionary with two indexes: `name` for the Drupal config name and `value` for the variable value.

    aegir_server_ssl_cipher_suite: 'EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH'
    aegir_server_ssl_protocol: 'All -SSLv2 -SSLv3'
    aegir_server_ssl_honor_cipher_order: 'On'
    aegir_server_ssl_compression: 'Off'
    aegir_server_ssl_use_stapling: 'On'

The SSL protocols and cipher suites that are used/allowed when clients make secure connections to your server. These are secure/sane defaults, but for maximum security, performand, and/or compatibility, you may need to adjust these settings. You may find some information in [Cipherli.st: Strong Ciphers for Apache, nginx and Lighttpd](https://cipherli.st/).

    aegir_server_ssl_extras: []

Here you can specify other vars for the configuration files of Apache SSL Servers. Each config must be a dictionary with two indexes: `name` for the Apache config name and `value` for the corresponding value.

    aegir_server_extras: []

Here you can specify other vars for the configuration files of Apache Servers. Each config must be a dictionary with two indexes: `name` for the Apache config name and `value` for the corresponding value.

    aegir_vhost_use_canonical_name: 'On'

With this configuration as `On`, Apache will use always `ServerName` as the host for every link it creates. Also, the php variable `$_SERVER['SERVER_NAME']` would have this value too.

    aegir_vhost_separate_logs: true

Generate different access and error log files for every site. By default, Aegir uses only an access file and an error file for all the sites.

    aegir_vhost_deflate: true

Enable Deflate mode in Apache.

    aegir_vhost_fileetag: true

Enable the use of FileETag headers.

    aegir_vhost_frame_options: SAMEORIGIN

This property set the security policy of the sites when they are loaded within a Frame or IFrame. You can check the valid options in [this Wikipedia Article](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Frame-Options).

    aegir_vhost_sts: true
    aegir_vhost_sts_max_age: 63072000
    aegir_vhost_sts_subdomains: true

HTTP Strict Transport Security is enabled by default, with a Max Age of 1 year and the subdomains are included.

    aegir_vhost_extras: []

Here you can specify other vars for the configuration files of Apache Virtual Hosts. Each config must be a dictionary with two indexes: `name` for the Apache config name and `value` for the corresponding value.

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

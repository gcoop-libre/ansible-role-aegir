# Ansible Role: Aegir

Installs Aegir, a control panel for deploying and managing large networks of Drupal sites.

## Requirements

A MySQL server is required. This server can be installed on the same machine, or a separate one (hence why this isn't listed as a dependency.) See the 'Example Playbook' section below for a simple method of installing a mysql server with Ansible. If Aegir's MySQL user and password are not defined, then the `mysql_root_username` and `mysql_root_password` are used.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

## Dependencies

  - getvalkyrie.drush (Installs Drush). Note that this role currently defaults
    to the Drush 7.x branch. As a result, Drupal 8 isn't supported by default.

## Example Playbook

    - hosts: servers
      roles:
        - { role: getvalkyrie.mysql }
        - { role: gcoop-libre.aegir }

After the playbook runs, the Aegir front-end site will be available, as will the Drush extensions (Provision, et. al.) that do the heavy lifting.

## License

MIT / BSD

## Author Information

This role was modified by [gcoop Cooperativa de Software Libre](http://gcoop.coop) in 2016.

It was originally created in 2015 by [Christopher Gervais](http://ergonlogic.com/), lead maintainer of the [Aegir Hosting System](http://www.aegirproject.org).

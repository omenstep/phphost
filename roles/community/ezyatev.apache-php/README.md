Ansible Simple Role: Apache - PHP
=========

This Ansible Simple Role install Apache and PHP. It also allow install php packages and apache modules.

To take in account:
* This role may be compatible with other operating systems than Ubuntu, but it hasn't been tested on no one else.
* This role has been tested on Ansible 1.8 but it could be compatible with lower version as well.

Requirements
------------

None.

Role Variables
--------------

This is **by default** configuration:

```yml
php_packages:
  - php

apache_modules:
  - rewrite
  - php
```

You can override these variable to get installed additional php modules or in order to enable other apache mods.

Moreover **you must** specify what the vhost filename is and a virtual host at least, for instance:

```yml
vhost_filename: my-awesome-domain.com.conf
apache_vhosts:
  - server_name: my-awesome-domain.com
    document_root: /var/www/my-awesome-domain.com/web
```

Additionally, by each vhost you can set SetEnv values, PHP ones and AliasMatch:

```yml
vhost_filename: my-awesome-domain.com.conf
apache_vhosts:
  - server_name: my-awesome-domain.com
    document_root: /var/www/my-awesome-domain.com/web
    directory_AllowOverride: All
    directory_allow_from: All
    directory_options: FollowSymLinks
    set_env:
      language: en
    php_value:
      newrelic.appname: my-awesome-domain.com
  - server_name: my-awesome-domain.es
    document_root: /var/www/my-awesome-domain.com/web
    set_env:
      language: es
    php_value:
      newrelic.appname: my-awesome-domain.es
    alias_match:
      ^(.*)$: /var/www/my-awesome-domain.com/web/index.php
```

That will end up with a single **my-awesome-domain.com.conf** vhost file and two virtual hosts inside.

**server_alias** also is allowed by each apache_vhosts:

```yml
vhost_filename: my-awesome-domain.com.conf
apache_vhosts:
  - server_name: my-awesome-domain.com
    server_alias: www.my-awesome-domain.com
    document_root: /var/www/my-awesome-domain.com/web
```

Dependencies
------------

None.

Example Playbook
----------------

Just add **ezyatev.apache-php** role in your playbook, set **vhost_filename** and one **apache_vhosts** at least and enjoy!

```yml
- hosts: webservers
  sudo: true
  roles:
    - role: ezyatev.apache-php
      vhost_filename: my-awesome-domain.com.conf
      apache_vhosts:
        - server_name: my-awesome-domain.com
          document_root: /var/www/my-awesome-domain.com/web
```

License
-------

MIT

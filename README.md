uWSGI Ansible Role
==================

This Ansible role will install uWSGI as needed from official repositories, manage apps with theirs UNIX sockets + ensure that required plugins are installed.


Requirements
------------

* Ansible 2.0+
* One of these target systems:
    * Debian Jessie & Stretch
    * Ubuntu 16.04


Supported plugins
-----------------

Some plugins such as Python or Ruby will require extra-specific configuration parameters in order to work as expected.

However, this role will not be immediately compatible will all ot those plugins, if the one that you want to use is not in the list, feel free to contribute.

Do not worry, this should be an easy step. See the "Contribution procedure" section to know how to get the job done.

There's a list of supported plugins for the moment :

- Rack Ruby 2.3 (`rack-ruby2.3`)


Usage example
-------------

This is a simple example of how to make a Rails app works. For other languages/frameworks, or extra parameters, read [default variables](defaults/main.yml) for further details.

```yaml
- role: uwsgi-app
  uwsgi_app_name: redmine
  uwsgi_app_socket_path: /var/run/redmine.sock
  uwsgi_app_chdir: /usr/share/redmine/
  uwsgi_app_plugin: rack-ruby2.3
  uwsgi_app_plugin_rack_ruby23_env: production
```

Simple as that.


Contribution procedure
----------------------

Debian-based operating systems generally already provide uWSGI and many of its plugins directly on the official repositories.

There's an example of supported plugins both by the official repos of Ubuntu 16.04 and Debian Stretch :

- `alarm-curl`
- `alarm-xmpp`
- `asyncio-python`
- `asyncio-python3`
- `curl-cron`
- `emperor-pg`
- `fiber`
- `gccgo`
- `geoip`
- `gevent-python`
- `glusterfs`
- `graylog2`
- `greenlet-python`
- `jvm-openjdk-8`
- `jwsgi-openjdk-8`
- `ldap`
- `lua5.1`
- `lua5.2`
- `luajit`
- `mono`
- `php`
- `psgi`
- `python`
- `python3`
- `rack-ruby2.3`
- `rados`
- `rbthreads`
- `ring-openjdk-8`
- `router-access`
- `servlet-openjdk-8`
- `sqlite3`
- `tornado-python`
- `v8`
- `xslt`


To implements one of those plugins into this role to make it fits your needs,
you need to do those steps :

### Step 1 : Update the README

You have seen the list of supported plugins by uWSGI upper, those are the identifiers of the plugins.

If for example you want to implements `rack-ruby2.3`, you will need to add in in the "Supported plugins" section of this README and make it respect this format :

- Rack Ruby 2.3 (`rack-ruby2.3`)

### Step 2 : Do researches

Search over the uWSGI documentation in order to find app parameters for the `.ini` file matching the plugins that you want to implements.

Then, create a file with the name of identifier in the directory `./templates/plugins/` of this role. For V8, it would be named `rack-ruby2.3.ini.j2`.

Once file created, fill it with plugin's required variables, the Ansible variables need to be prefixed with `uwsgi_app_plugin_rack_ruby23_`.

Note that `rack-ruby2.3` became `rack_ruby23` for example purposes : In your case, take the plugin indentifier, remove the dots (if there's any) and replace dashes with underscores.

Please be the more generic possible so the role will be useable in the most possible cases.

### Step 3 : Update the defaults

Open the `./defaults/main.yml` file, add a section in the same format of what's thats already there, and fill it with your Ansible variables that are prefixed with `uwsgi_app_plugin_rack_ruby23_` (do not forget to replace `rack_ruby23` with your own modified plugin identifier).

### Step 4 : Test it

Implements what you've done into your playbook, and then test it, if it works go to the next paragraph, else, play around with it until it works like a charm.

### Step 5 : Pull request

Do your pull request on GitHub !


Conclusion
----------

We absolutely appreciate patches, feel free to contribute directly on the GitHub project.

Repositories / Development website / Bug Tracker:
- https://github.com/savoirfairelinux/ansible-uwsgi-app.git

Do not hesitate to join us and post comments, suggestions, questions and general feedback directly on the issues tracker.

**Website :** https://www.savoirfairelinux.com/

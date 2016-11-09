# Convivio Virtual Machine

This is the configuration repository for the Convivio Virtual Machine (CVM). CVM is a Composer installation and configuration of [Drupal VM](https://github.com/geerlingguy/drupal-vm) by [@geerlingguy](https://github.com/geerlingguy).

The configuration follows the documentation for [installing Drupal VM as a Composer dependency](http://docs.drupalvm.com/en/latest/other/drupalvm-composer-dependency/).

### Requirements

CVM installs the latest Drupal VM (3.5.0 currently - see [Drupal VM Releases](https://github.com/geerlingguy/drupal-vm/releases)), so the installation requirements are the same as that. To install and work with CVM you must have:

- Ansible 2.2.x
- Vagrant 1.8.6+
- VirtualBox 5.1.6+

### What's in the CVM box?

Install CVM and you get a local development virtual machine running on Vagrant that installs:

- Centos 7
- Nginx
- Varnish
- Memcached
- Redis
- Mailhog

All of this very configurable, and if you prefer to have a different local installation that is easily achieved with some simple local configurations.

# Install CVM

Installing CVM is dead easy.

1. Clone the repo
2. Run `composer install` to install Drupal VM ready to run
3. If you want to make any changes to your own, then copy `config/default.local.config.yml` to `config/local.config.yml` add them in that file. Here, you can override or extend any of the variables set in `config/config.yml`.
4. Run `vagrant up`
  - The vagrant should run and install all the bits and pieces to give you a working virtual machine for your own local development.
5. Boil the kettle for a nice cup of tea while the installer runs.
  - It shouldn't take _too_ long to install all the things, but a watched pot never boils so use the time to get a lovely cup of tea.

# Working with CVM

CVM mounts the directory `~/Sites` on your host machine into `/var/www` (determined with the `drupal_composer_site_install_dir` config variable) on the virtual machine. This makes it easy to run an Nginx host for any project in your `~/Sites` directory.

You are then free to configure your projects to work in any way you see fit - built with Composer, with make files, by hand, or whatever. You'll just need to set up an Nginx host for it, and maybe a database too.

## Create an Nginx host

Just add a few lines into config/local.config.yml under the `nginx_hosts:` variable, following the pattern of the previous entry/entries. E.g.

```
nginx_hosts:
  - server_name: "www.demo.dev"
    root: "{{ drupal_composer_site_install_dir }}/demo/web"
    is_php: true
  - server_name: "www.mytestsite.dev"
    root: "{{ drupal_composer_site_install_dir }}/mytestsite/docroot"
```

## Create a database for your site

As with an Nginx host, you can easily add a database by just adding a few lines into config/local.config.yml under the `mysql_databases:` variable, following the pattern of the previous entry/entries. E.g.

```
mysql_databases:
  - name: "{{ drupal_db_name }}_demo"
    encoding: utf8
    collation: utf8_general_ci
  - name: "{{ drupal_db_name }}_mytestsite"
    encoding: utf8
    collation: utf8_general_ci
```

The config variable `drupal_db_name` has the value 'drupal', so this format will give your locals DBs a prefix of 'drupal_'.

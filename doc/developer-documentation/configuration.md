
# Configuration

BackBee CMS is highly customizable. In this section, all the available configurations are broken down by the key (e.g. doctrine) that defines each possible section of your application configuration.

## Bootstrap configuration (bootstrap.yml)

This file is your application entry point so it **must exist** and be located in `repository/Config/` folder. Let's explore together every parameter of this file:

* `debug`: (type: bool) enables debug. If **true**, the container is generated on each request and the web profiler is also visible.
* `container`: (type: array)
    * `dump_directory`: (type: string) define the location of the generated container file.
    * `autogenerate`: (type: bool) define the default value for container and cache autogeneration parameters (respectively `container.autogenerate` and `bbapp.cache.autogenerate`)

Note that the parameter`container.autogenerate` has no impact on the container regeneration. To do so,  you have to delete manually the dumped file. The other way to regenerate the container is to update the `bootstrap.yml`. Indeed, the last modification date property of `bootstrap.yml` is used to compute container dump filename.

## Bundles configuration (bundles.yml)

To activate a bundle, you need to add it in the `repository/Config/bundles.yml` file:

```yaml
# repository/Config/bundles.yml
demo: BackBee\Bundle\DemoBundle\Demo
toolbar: BackBee\Bundle\ToolbarBundle\Toolbar
```

The bundle name (e.g. `demo` or `toolbar`) is important because you will need it to retrieve your bundle:

```php
<?php

require_once __DIR__ . '/vendor/autoload.php';

$app = new BackBee\BBApplication();

$app->getBundle('toolbar'); // returns the instance of BackBee\Bundle\ToolbarBundle\Toolbar
```

You can learn more about bundle configuration on [its documentation](/developer-documentation/components/bundle/).

## Database configuration (doctrine.yml)

BackBee relies on Doctrine 2 ORM to handle its interactions with the database. So you have to tell Doctrine how to connect to your database by adding the `repository/Config/doctrine.yml` file (you can use `repository/Config/doctrine.yml.dist` to help you to write your own `doctrine.yml`).

```yaml
# /repository/Config/doctrine.yml
dbal:
    driver: pdo_mysql
    host: localhost
    port: 3306
    dbname: backbee_standard
    user: root
    password: ~
    charset: utf8
    collation: utf8_general_ci
    defaultTableOptions:
        collate: utf8_general_ci
        engine: InnoDB
        charset: utf8
    orm:
        metadata_cache_driver: # see repository/Config/services.yml.dist to enable cache of class metadata
            type: service
            id: @doctrine.metadata.cache.driver
```

Although BackBee supports SQLite, we highly recommend you to use exclusively the MySQL driver. The SQLite one is used to run tests.

## Security Configuration (security.yml)

You can set up firewalls and sudoers inside this file. A firewall defines secured area while a sudoer is a super administrator which means that he has every right on any secured action. When you install BackBee via the Standard Edition, the first user you register during the installation process will be your first sudoer.

```yaml
# repository/Config/security.yml
firewalls:
    firewall_name: { pattern: ^/, anonymous: null }
sudoers:
    admins: 1
```

See below how you can the user `foobar` (id = `8000`) as sudoer:

```yaml
# repository/Config/security.yml
firewalls:
    firewall_name: { pattern: ^/, anonymous: null }
sudoers:
    admins: 1
    foobar: 8000
```

Note that this configuration file does not create new user for you. The user `foobar` must exist in database and the identifier `8000` must be associated to `foobar`.

You can learn more about the security component by accessing to [its dedicated documentation](/developer-documentation/components/security/). As BackBee use Symfony's security component, you can also get more about it on [its official documentation](http://symfony.com/doc/current/book/security.html).

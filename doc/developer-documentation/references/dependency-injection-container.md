# Dependency Injection Container

The Dependency Injection Container (DIC) allows you to standardize and centralize the way objects are constructed. BackBee CMS relies on this container.

## Basic usage

To get BackBee's service container, you can use the method `BackBee\BBApplication::getContainer()`. You can retrieve services and parameters by calling `::get()` and `::getParameter()` on `BackBee\DependencyInjection\Container`.

```php
# __PROJECT_ROOT_DIR__/index.php
<?php

require __DIR__ . '/vendor/autoload.php';

use BackBee\BBApplication;

$app = new BBApplication();

$container = $app->getContainer(); // returns application's dependency injection container

$container->get('classcontent.manager'); // returns an instance of BackBee\ClassContent\ClassContentManager

$container->getParameter('debug'); // returns true or false, depending on your configuration
```

You can see all parameters and services registered by BackBee by exploring every YAML files inside the `Config/services/` folder (or `vendor/backbee/backbee/Config/services/` if you're using BackBee Standard Edition).

## To register new service and parameter

The easiest way for you to register a new service or a parameter inside BackBee's service container is to use configuration files. Even if XML and YAML formats are accepted, we highly recommend to use YAML for its readability and simplicity. To learn about YAML syntax at [its official website](http://yaml.org/).

The settings file must be named `services.yml` and be in one of these locations:

- `__PROJECT_ROOT__/repository/Config/services.yml` (for BackBee Standard Edition)
- `__BUNDLE_ROOT__/Config/services.yml` (for any bundle)

### Register a parameter

To register a new parameter inside the container, you simply have to declare it inside `parameters` section:

```yaml
# repository/Config/services.yml
parameters:
    name: world
    message: hello %name%!
```

Note that you can reference others parameters  with the `%%` syntax. In the example above, `%name%` will be replaced by `world`:

```php
# __PROJECT_ROOT_DIR__/index.php
<?php

require __DIR__ . '/vendor/autoload.php';

use BackBee\BBApplication;

$app = new BBApplication();

$container = $app->getContainer();

echo $container->getParameter('name'); // print "world"
echo $container->getParameter('message'); // print "hello world!"
```

### Register a service

To add a new service inside the DIC, you have to use the `services` section to do so:

```yaml
# repository/Config/services.yml
parameters:
    current_datetime.class: \DateTime

services:
    current_datetime:
        class: %current_datetime.class%
        arguments: ["now"]
```

According to the service `current_datetime` declaration above, the DIC will instantiate the service (`new \DateTime('now')`) and return it:

```php
# __PROJECT_ROOT_DIR__/index.php
<?php

require __DIR__ . '/vendor/autoload.php';

use BackBee\BBApplication;

$app = new BBApplication();

$app->getContainer()->get('current_datetime'); // return an instance of \DateTime
```

### Inject a service or a parameter inside a service constructor

You can decide to inject a service and/or a parameter inside another service constructor by using the syntax `@__IDENTIFIER__`. To illustrate this, we will use the following `Foobar` class:

```php
<?php

class Foobar
{
    private $datetime;
    private $name;

    public function __construct(\DateTime $datetime, $name)
    {
        $this->datetime = $datetime;
        $this->name = $name;
    }
}
```

As you can see, the class `Foobar` expects an instance of `DateTime` as first parameter. So let's add the `foobar` service inside the service container by editing the `services.yml` file:

```yaml
# repository/Config/services.yml
parameters:
    name: John Do
    current_datetime.class: \DateTime

services:
    current_datetime:
        class: %current_datetime.class%
        arguments: ["now"]

    foobar:
        class: \Foobar
        arguments: ["@current_datetime", "%name%"]
```

Like we show it above, `@current_datetime` means that we inject an object as first parameter of `Foobar` constructor (`current_datetime` which is an instance of `DateTime` so everything is fine).

## Learning more

If you take a look at `BackBee\DependencyInjection\Container` API, you will notice that you can use `::set()` method to add new services to the service container. But for performances reasons, BackBee will dump and freeze its service container (including all services and parameters). It occurs on the `bbapplication.init` event so it happens very early (at the end of `BackBee\BBApplication::__construct`). It's not possible to add new service on a frozen container. That why we recommend you to use YAML configurations files to declare your services (in fact you can set new services on a frozen container but they must be declared as synthetic).

BackBee dependency injection container is based on Symfony DependencyInjection component. To learn more about synthetic services, services tags or services factories, we invite you to take a look at [the official documentation](http://symfony.com/doc/2.7/components/dependency_injection/index.html). Except to the "Compiling" and "Compiler Pass" features, everything else are availables.

# Events

Let's explore every option you can use in a `events.yml` file. Before we start, find below every valid location for your events file:

- In bundle: `__BUNDLE_ROOT_DIR__/Config/events.yml`
- In BackBee core: `__BACKBEE_ROOT_DIR__/Config/events.yml`
- In BackBee Standard Edition: `__PROJECT_ROOT_DIR__/repository/Config/events.yml`

If you want to load your events in a custom way, you should take a look to [the bundle component documentation](/developer-documentation/components/bundle/).

## Configuration

```yaml
# Config/events.yml of backbee/backbee sample

classcontent.onflush:
    listeners:
        - [BackBee\ClassContent\Listener\ClassContentListener, onFlushContent]
        - [BackBee\ClassContent\Listener\RevisionListener, onFlushContent]
        - [BackBee\ClassContent\Listener\IndexationListener, onFlushContent]
        - [@cache.listener, onFlushContent]
        - [BackBee\Event\Listener\RewritingListener, onFlushContent]
        - [BackBee\MetaData\Listener\MetaDataListener, onFlushContent, -100]
```

### Event name (e.g. `classcontent.onflush`)

When you want to register an event listener, the first step is to define which event you want to listen to. In the sample above, we added a listener to the `classcontent.onflush` event.

### Callbacks

An event listener callback is a callable array containing at least 2 elements:

- The first argument is a fully-qualified class name (e.g. `BackBee\ClassContent\Listener\ClassContentListener`)
- The second argument is the method to call (e.g. `onFlushContent`)

 Note that BackBee does not instantiate the event listener class. It's invoked statically.

If you need to apply a specific behavior before your event listener is actually handling the event, you can specify a service identifier from the Dependency Injection Container (e.g. `@cache.listener` - remember to target the service identifier by using the `@` character).

### Priority

You can leverage your event listener's priority using Symfony's Event Dispatcher system, which BackBee extends.

- Event listener's default priority is set to `0`
- The highest your priority, the earliest your event is triggered
- When two events listeners have the same priority, their declaration in the execution pile determine which one is called first

#### Basic usage

```yaml
random_event.ready:
    listeners:
        - [Foo\BarListener, onReady]
        - [Hello\WorldListener, onReady]
        - [Open\SourceListener, onReady]

# listeners call order on dispatch of random_event.ready:

1. Foo\BarListener::onReady
2. Hello\WorldListener::onReady
3. Open\SourceListener::onReady
```

#### Usage with priority

```yaml
random_event.ready:
    listeners:
        - [Foo\BarListener, onReady, -100]
        - [Hello\WorldListener, onReady]
        - [Open\SourceListener, onReady, 100]

# listeners call order on dispatch of random_event.ready:

1. Open\SourceListener::onReady
2. Hello\WorldListener::onReady
3. Foo\BarListener::onReady
```

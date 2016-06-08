# Create new templating helpers

This recipe will deal about `Renderer` helpers. A helper must always return a string. For example, you should use them when you have to generate a string depending on objects and/or resources that are not accessible from views.

To write your own helper, the PHP class must extend `BackBee\Renderer\Helper\AbstractHelper` and  contain the `__invoke()` method. By doing it this way, your helper name is the class name. You don't need to worry about how to autoload it as long as it's located in `repository/Templates/helpers` or in `Templates/helpers` of bundles.

## Usage

Let's create a helper `sayHello` that says 'Good morning you!', 'Good afternoon you!' or 'Good evening you!' depending on current time. If the user is logged in 'you' will be replaced by user's first name.

Let's write the PHP class (`repository/Templates/helpers/sayHello.php`):

```php
<?php

namespace BackBee\Renderer\Helper;

/**
 * @author Eric Chau <eric.chau@lp-digital.fr>
 */
class sayHello extends AbstractHelper
{
    const MORNING_MARK = 12;
    const AFTERNOON_MARK = 20;

    /**
     * Returns hello message depending on current time and if the user is connected.
     *
     * @return string
     */
    public function __invoke()
    {
        $name = 'you';
        $token = $this->_renderer->getApplication()->getBBUserToken();
        if (null !== $token) { // if $token is not null, the user is authenticated
            $name = $token->getUser()->getFirstname();
        }

        $momentLabel = 'evening';
        $hour = date('H');
        if (self::MORNING_MARK > $hour) {
            $momentLabel = 'morning';
        } elseif (self::AFTERNOON_MARK > $hour) {
            $momentLabel = 'afternoon';
        }

        return "Good {$momentLabel} {$name}!";
    }
}
```

Here's how you can use `sayHello` helper in a template below:

```twig
# repository/Templates/scripts/random_view.html.twig
<h2>{{this.sayHello()}}</h2>
```

Assuming that it's currently 8:00 am and you are logged in using account with Eric as first name, the result will be:

```
<h2>Good morning Eric!</h2>
```

And if it's 9:00 pm and you are not connected:

```
<h2>Good evening you!</h2>
```

Well, this recipe is over. You should now be able to create by yourself your helpers to do some amazing things!

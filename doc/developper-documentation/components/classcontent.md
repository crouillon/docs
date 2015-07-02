# Components

## The ClassContent component


## ClassContent Parameters

Parameters allow to configure contents. ClassContent parameters is used usually in the listener but can be used everywhere. ClassContent parameters are contributed in back office by the user with the gearing button of Block.

Parameters is builded from the block's YAML like

```yaml
BlockDemo:
    properties:
        name: Block demo
        description: "Block for demonstration purposes"
        category: [Demo]
    elements:
        text:
            type: BackBee\ClassContent\Element\Text
    parameters:
        mytext:
            label: 'My text'
            type: 'text'
            value: ''
        myselect:
            label: 'My select'
            type: 'select'
            options:
                'foo': 'Foo'
                'bar': 'Bar'
            value: ['foo']

```

### Use parameters

If ClassContent Parameters are not validated by user the content parameters are overriden by revision parameters.
**Only value of the parameter is saved.**

To get default parameters:
```php
<?php
$params = $content->getDefaultParams();

/** That returns all parameters with definitions and default values:
 *  array (size=2)
 *    'mytext' =>
 *      array (size=3)
 *        'label' => string 'My text' (length=7)
 *        'type' => string 'text' (length=4)
 *        'value' => string '' (length=0)
 *    'myselect' =>
 *      array (size=4)
 *        'label' => string 'My select' (length=9)
 *        'type' => string 'select' (length=6)
 *        'options' =>
 *          array (size=2)
 *            'foo' => string 'Foo' (length=3)
 *            'bar' => string 'Bar' (length=3)string 'yolo' (length=4)
 *        'value' =>
 *          array (size=1)
 *            0 => string 'foo' (length=3)
 */
```

To get all parameters :
```php
<?php

$params = $content->getAllParams();

/** That returns all parameters with definitions and new values
 *  array (size=2)
 *    'mytext' =>
 *      array (size=3)
 *        'label' => string 'My text' (length=7)
 *        'type' => string 'text' (length=4)
 *        'value' => string '' (length=0)
 *    'myselect' =>
 *      array (size=4)
 *        'label' => string 'My select' (length=9)
 *        'type' => string 'select' (length=6)
 *        'options' =>
 *          array (size=2)
 *            'foo' => string 'Foo' (length=3)
 *            'bar' => string 'Bar' (length=3)
 *        'value' =>
 *          array (size=1)
 *            0 => string 'foo' (length=3)
 */
```

To get one param:

```php
<?php
$param = $content->getParam('mytext');

/** That returns the definition and new value
 *  array (size=3)
 *    'label' => string 'My text' (length=7)
 *    'type' => string 'text' (length=4)
 *    'value' => string '' (length=0)
 */
```

To get the value of the param:
```php
<?php
$paramValue = $content->getParamValue('mytext');

// That returns the new value string '' (length=0)
```

To set a value, you need to set up the same type you declare in the YAML file as value:
```php
<?php
$content->setParam('mytext', 'foo');
$param = $content->getParamValue('mytext')

// That returns a new value set  string 'foo' (length=3)
```

## Parameters reference

* checkbox
* datetimepicker
* hidden
* linkSelector
* mediaSelector
* nodeSelector
* password
* radio
* select
* text
* textarea

All parameters has default options:

#### Default options

 |   | Type | Description | Default | Mandatory
--- | --- | --- | --- | ---
**label** |  String | Label is displayed above the field | Empty | No
**value** | Mixed | This field must be set and allows to describe the  parameter's type | None | Yes |
**type** | String | Key of one of available parameters | None | Yes |

***

#### Checkbox

Like HTML checkbox attribute

 |   | Type | Description | Default | Mandatory
--- | --- | --- | --- | ---
**options** |  Array | List of options (key/value) | Empty array | Yes
**inline** | Boolean | Display checkbox inline | false | No |
**value** | Array | List of selected checkboxes | Empty array | Yes |

**Example**:

```yaml
parameters:
    mycheckbox:
        type: 'checkbox'
        options:
            'foo': 'Foo'
            'bar': 'Bar'
        value: ['foo']
        inline: true
```

```php
<?php
$param = $content->getParamValue('mycheckbox'); // sarray('foo')
```
***

#### Datetime picker

Text element with a datetimepicker

 |   | Type | Description | Default | Mandatory
--- | --- | --- | --- | ---
**value** | Number | It is an timestamp value | Empty | Yes |

**Example**:

```yaml
parameters:
    mydatetimepicker:
        type: 'datetimepicker'
        value: 1435573740
```

```php
<?php
$param = $content->getParamValue('mydatetimepicker'); // 1435573740
```

***

#### Hidden

Like HTML hidden attribute

 |   | Type | Description | Default | Mandatory
--- | --- | --- | --- | ---
**value** | String | The default text  | Empty | Yes |

**Example**:

```yaml
parameters:
    myhidden:
        type: 'hidden'
        value: 'foo'
```

```php
<?php
$param = $content->getParamValue('myhidden'); // foo
```

***

#### Link selector

Link selector allows to choose a link from one of your website pages or an external link.

 |   | Type | Description | Default | Mandatory
--- | --- | --- | --- | ---
**value** | Json | Object which contains url / title / pageUid / target  | Empty array | Yes |

**Example**:

```yaml
parameters:
    mylinkselector:
        type: 'linkSelector'
        value: []
```

```php
<?php
# Example with select one link
$param = $content->getParamValue('mylinkselector');
// array('url' => '/foo', 'title' => 'Foo', 'pageUid' => 'anPageUid', 'target' => '_self')
```
** Page tree **:
We recommend to retrieve the url from the page entity instead of the url attribute because the url can change.

** External link ** :
In this case, note the pageUid attribute is (obviously) null.

***

#### Media selector

Media selector allow to choose a list of media in the media library.

 |   | Type | Description | Default | Mandatory
--- | --- | --- | --- | ---
**value** | Json | Object which contains folder_uid / image / media_id / title /type / uid  | Empty array | Yes |

**Example**:

```yaml
parameters:
    mymediaselector:
        type: 'mediaSelector'
        value: []
```

```php
<?php
# Example with select one media
$param = $content->getParamValue('mymediaselector');
// array(array('folder_uid' => 'anFolderUid', 'image' => 'imageUrl', 'media_id' => '1', 'title' => '_Foo', 'type' => 'Media/Image', 'uid' => 'anUid'))
```

***

#### Node selector

Node selector allows to choose a page from tree

 |   | Type | Description | Default | Mandatory
--- | --- | --- | --- | ---
**value** | Json | Object which contains url  pageUid / title  | Empty array | Yes |

**Example**:

```yaml
parameters:
    mynodeselector:
        type: 'nodeSelector'
        value: []
```

```php
# Example of node selection
$param = $content->getParamValue('mynodeselector');
# array('pageUid' => 'anPageUid', 'title' => 'Foo')
```
We recommend to retrieve the title from the page entity instead of the title attribute because the title can change.

***

#### Password

Like HTML password attribute

 |   | Type | Description | Default | Mandatory
--- | --- | --- | --- | ---
**value** | String | The default text | Empty | Yes |

**Example**:

```yaml
parameters:
    mypassword:
        type: 'password'
        value: 'foo'
```

```php
<?php
$param = $content->getParamValue('mypassword'); // foo
```
***

#### Radio

Like HTML radio attribute

 |   | Type | Description | Default | Mandatory
--- | --- | --- | --- | ---
**options** |  Array | List of options (key/value) | Empty array | Yes
**inline** | Boolean | Display radio inline | false | No |
**value** | Array | List of selected radio | Empty array | Yes |

**Example**:

```yaml
parameters:
    myradio:
        type: 'radio'
        options:
            'foo': 'Foo'
            'bar': 'Bar'
        value: ['foo']
        inline: true
```

```php
<?php
$param = $content->getParamValue('myradio'); // array('foo')
```
***

#### Select

Like HTML select attribute

 |   | Type | Description | Default | Mandatory
--- | --- | --- | --- | ---
**options** |  Array | List of options (key/value) | Empty array | Yes
**value** | Array | List of selected radio | Empty array | Yes |
**multiple** | Boolean | Allows multiple selection | false | No

**Example**:

```yaml
parameters:
    mypassword:
        type: 'select'
        options:
            'foo': 'Foo'
            'bar': 'Bar'
        value: ['foo']
        multiple: true
```

```php
<?php
$param = $content->getParamValue('myradio'); //array('foo')
```

***

#### Text

Like HTML text attribute

 |   | Type | Description | Default | Mandatory
--- | --- | --- | --- | ---
**value** | String | The default text | Empty | Yes |

**Example**:

```yaml
parameters:
    mytext:
        type: 'text'
        value: 'foo'
```

```php
<?php
$param = $content->getParamValue('mypassword'); // foo
```
***

#### Textarea

Like HTML textearea attribute

 |   | Type | Description | Default | Mandatory
--- | --- | --- | --- | ---
**value** | String | The default text | Empty | Yes |
**rows** | String | the rows html attribute | 5 | No

**Example**:

```yaml
parameters:
    mytextearea:
        type: 'textarea'
        value: 'BackBee'
        rows: 10
```

```php
<?php
$param = $content->getParamValue('mytextearea'); // BackBee
```
***

### Special parameters

#### Rendermode

Rendermode parameters allow to list automatically rendermodes of the content and use them directly.

 |   | Type | Description | Default | Mandatory
--- | --- | --- | --- | ---
**value** | Array | Selected rendermode | Empty | Yes |

```yaml
parameters:
    rendermode:
        type: 'select'
        value: []
```

**The key must be 'rendermode'**

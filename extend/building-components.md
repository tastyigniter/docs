---
title: "Building a Component"
section: extend
sortOrder: 210
---

## Introduction

In this section, you will learn how to build component and not how to attach [components](../customize/components) to theme templates. Building a component involves creating the class of your component by extending the `BaseComponent` class and some of its methods, then [**registering your component**](#component-registration) to be displayed on your site and available from the Admin Interface.

Components live in the **/components** subdirectory of an extension directory.

```yaml
extensions/
  igniter/
    helloworld/         		<=== Extension directory
      components/				<=== Components subdirectory
        block/       			<=== Component partials directory
          default.blade.php        	<=== Component default partial (optional)
        HelloBlock.blade.php    		<=== Component class file
```

## Defining the component

To begin, you can either use your preferred file manager to create files and directory for the **Block** component or simply call the following command from the application directory to generate a component with basic files and directories:

```bash
php artisan create:component Igniter.HelloWorld Block
```

### Component class

The component class file should extend the `\System\Classes\BaseExtension` base class and define the functionality and properties of the component. The following is an example defines the `Block` component:

```php
namespace Igniter\HelloWorld\Components;

class Block extends \System\Classes\BaseComponent
{
   	public function defineProperties()
    {
        return [];
    }
    
    public function alerts()
    {
        return ['Success Alert', 'Warning Alert', 'Danger Alert'];
    }
}
```

The component properties and methods are available on the page or layout its attached through the component variable
which corresponds to the alias of the component. For example, if the `HelloBlock` component was defined on a page or
layout as `'[helloBlock]'`, you will be able to access its `alerts` through the `$helloBlock` variable:

```php+HTML
@foreach ($helloBlock->alerts() as $message) {
    <p>{{ $message }}</p>
@endforeach
```

### Component registration

Components must be registered by overriding the `registerComponents` method within the [Extension registration class](extensions#registration). This lets the app know about the component and gives it an **alias name** for its use within themes. To register the `Block` component with the default alias name **helloBlock**, you may override the `registerComponents` method:

```php
public function registerComponents()
{
    return [
        'Igniter\HelloWorld\Components\Block' => [
            'code' => 'helloBlock',
            'name' => 'Name of the hello block component',
            'description' => 'Description of the hello block component',
        ],
    ];
}
```

> More information on using components can be found in the [Components article](../customize/components).

### Component properties

Components can be configured using properties defined for each component by overriding the `defineProperties` method. Let's define a **maxItems** property to limit the number of alerts allowed

```php
public function defineProperties()
{
    return [
        'maxItems' => [
             'title'      	=> 'Max items',
             'description'  => 'The most number of alert items allowed',
             'default'      => 10,
             'type'         => 'text',
        ]
    ];
}
```

The method should return an array with the property keys as indexes and the property parameters as values. The property keys are used to access the component property values within the component class.

| Key             | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| **label**       | required, the property label, it is used by the component Selector in the Admin Interface. |
| **type**        | optional, specifies the property type. The type defines the form field type. Currently supported types are **<br/>text**, **number**, **checkbox**, **radio**, **select** and **selectlist**. Default value: **text**. |
| **default**     | optional, the default property value.                        |
| **comment**     | optional, the property description, it is used by the component Selector in the Admin Interface. |
| **placeholder** | optional placeholder for text and select properties.         |
| **options**     | optional array of options for checkbox, radio, select, selectlist properties. |


The **options** property key can be static or dynamic. Using the `maxItems` property, let's define static options:

```php
public function defineProperties()
{
    return [
        'maxItems' => [
            ...
            'type'   => 'select',
            'options' => [
                '20' => '20 Per page', 
                '50' => '50 Per page'
            ]
        ]
    ];
}
```

Dynamically, the options can be fetched by omitting the `options` key from the property definition and defining a method that returns the list of options. The method name should be the studly cased name of the property. For example, `getPropertyOptions` where **Property** is the name of the property. In this example, we'll define a method for the `maxItems` property.

```php
public function defineProperties()
{
    return [
        'maxItems' => [
            ...
            'type'   => 'select',
        ]
    ];
}

public function getMaxItemsOptions()
{
    return [
        '20' => '20 Per page', 
        '50' => '50 Per page'
    ];
}
```

Reading the property value from within the **component class**:

```php
// Get a single value
$maxItems = $this->property('maxItems');

// Get a value and return a default value if it doesn't exist
$maxItems = $this->property('maxItems', 8);

// Get all properties as array
$properties = $this->getProperties();
```

Accessing the property value from a **component partial**:

```php
{{ $__SELF__->property('maxItems') }}
```

### Component partials

Besides the default partial, components can also provide additional partials which can be used within other template files or in the default partial itself. 

```yaml
extensions/
  igniter/
    helloworld/
      components/
        partials/				<=== Component shared partials directory
          shared.blade.php			<=== Component shared partial file
        block/ 
          default.blade.php
          pagination.blade.php        <=== Component additional partial
        HelloBlock.blade.php    		<=== Component class file
```

> Components can share partial files by placing them in the **components/partials** subdirectory of the extension directory. 

### Component initialization

Sometimes you might want to handle some initialization logic before AJAX handlers and before the execution cycle of the page. You can override the `initialize` method in the component class to handle such initialization logic.

### Component class methods

The following methods are supported in the component class:

| Method              | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| **initialize()**    | initialization method, called when the component is attached to a page or layout |
| **onRun()**         | override to hook into the page's execution life cycle.       |
| **onRender()**      | override to handle specific logic before rendering the default partial of the component. |
| **renderPartial()** | render a specified component partial or shared partial.      |

## Page execution cycle handlers

Every time the page loads, the Main Controller executes the `onRun` method of the attached components. Overriding the method in the component class allows components to hook into the page execution cycle. Let's inject some variables into the page:

```php
public function onRun()
{
    $this->page['var'] = 'value';
}
```

Handler functions can be defined in the layout and page [PHP code section](../customize/themes#php-code-section) and component classes. These handlers are executed in the following sequence:

- `onInit` layout method.
- `onInit` page method
- `onStart` layout method
- `onRun` layout components method
- `onStart` page method
- `onRun` page components method
- `onEnd` page method
- `onEnd` layout method

## Defining AJAX handlers

Components class can define AJAX event handlers can be defined in the component class as they can be defined in the page or layout [PHP code section](../customize/themes#php-code-section).

```php
public function onAddItem()
{
    $value1 = post('value1');
    $value2 = post('value2');
    $this->page['result'] = $value1 + $value2;
}
```

If the alias for this component was `helloBlock` this handler can be accessed by `helloBlock::onAddItem`.

> Please see the [Handling AJAX Requests](../advanced/ajax-request) article for more details.

## Inject page assets

Use the `addCss` and `addJs` controller methods to add assets to the pages or layouts the components are attached to:

```php
public function onRun()
{
    // Assets path that begins with a slash (/) is relative to the website root directory
    $this->addJs('/extensions/igniter/helloworld/assets/js/controls.js');

    // Paths that does not begin with a slash is relative to the component directory
    $this->addJs('assets/js/controls.js');
}
```


---
title: "Widgets"
section: extend
sortOrder: 220
---

## Introduction

In TastyIgniter, widgets are self-contained blocks of functionality used to perform specific tasks within the Admin Interface. Widgets always have a user interface and admin controller, referred to as the widget class. The widget class is responsible for preparing data for the widget and handling AJAX requests that are initiated from the widget's user interface.

## Generic widget

Widgets in TastyIgniter are the admin equivalent of frontend [components](../extend/components). The major difference is that admin widgets use PHP arrays for configuration and are linked specifically to admin pages.

Widget classes reside in the `src/Widgets` directory of an extension. Widgets may include assets and partials to enhance functionality. Here's an example of a typical widget directory structure:

```yaml
author/
  extension/
    src/
      Widgets/
        MyWidget.php
    resources/
      css/
        mywidget.css
      js/
        mywidget.js
      views/
        _partials/
          widgets/
            mywidget.blade.php
```

### Widget class

The widget class is a PHP class that extends the `\Igniter\Admin\Classes\BaseWidget` class. The class contains properties and methods that define the widget's behavior and appearance. Here is an example of a simple widget class:

```php
namespace Author\Extension\Widgets;

class MyWidget extends \Igniter\Admin\Classes\BaseWidget
{
    public string $defaultAlias = 'mywidget';

    public function render()
    {
        $this->vars['var'] = 'value';
        
        return $this->makePartial('mywidget');
    }
}
```

The widget class must implement a `render()` method that returns the widget's markup. In this example, the `makePartial()` method will scan the `resources/views/widgets` or `resources/views` directory of an extension to render the `mywidget.blade.php` view file. The `$vars` property is used to pass data to the view file. Alternatively you may pass the variables to the second parameter of the `makePartial()` method:

```php
return $this->makePartial('mywidget', ['var' => 'value']);
```

### AJAX handlers

Widgets in TastyIgniter implement the same AJAX handling approach as [admin controllers](../extend/controllers). Widgets can handle AJAX requests by defining methods that start with `on` followed by the AJAX handler name. For example, to handle an AJAX request with the handler name `onAction`, you would define a method named `onAction` in the widget class. Here's an example of an AJAX handler method:

```php
public function onAction()
{
    // Your AJAX handler logic here
}
```

The only different between widget AJAX handlers and those of admin controllers is how they are referenced. When referring to a widget AJAX handler within widget partials, you should use the widget's `getEventHandler()` method to accurately return the handler name specific to the widget. For example:

```blade
<button
    type="button"
    data-request="{{ $this->getEventHandler('onAction') }}"
>Click me</button>
```

The example above will generate the following HTML attribute value by prefixing the handler name with the widget's alias:

```html
<button
    type="button"
    data-request="mywidget::onAction"
>Click me</button>
```

### Binding widgets to controllers

Before you can use a widget on admin pages in TastyIgniter, it must be bound to an admin controller. This is done using the widget's `bindToController` method. The optimal place to initialize a widget is within the constructor of the controller. For example:

```php
public function __construct()
{
    parent::__construct();
    
    $myWidget = new MyWidget($this);
    $myWidget->bindToController();
}
```

Once a widget is bound to a backend controller in TastyIgniter, you can access it within the controller's view or partial using its assigned alias.

```blade
{{ $this->widgets['mywidget']->render() }}
```

## Using form widget

Form widgets in TastyIgniter allow you to introduce new control types to admin forms. To use form widgets, they must first be registered in the [Extension class](../extend/extensions#extension-class).

Form widget classes are located in the `src/FormWidgets` directory of an extension. Form widgets can also include assets and partials. Here's an example:

```yaml
author/
  extension/
    src/
      FormWidgets/
        MyFormWidget.php
    resources/
      css/
        myformwidget.css
      js/
        myformwidget.js
      views/
        _partials/
          formwidgets/
            myformwidget.blade.php
```

### Form widget class

The form widget class is a PHP class that extends the `\Igniter\Admin\Classes\BaseFormWidget` class. A registered widget can be used in the admin [form definition file](../extend/forms#form-definition-file). Here is an example of a simple form widget class:

```php
namespace Author\Extension\FormWidgets;

class MyFormWidget extends \Igniter\Admin\Classes\BaseFormWidget
{
    public string $defaultAlias = 'myformwidget';

    public function render()
    {
        $this->vars['var'] = 'value';
        
        return $this->makePartial('myformwidget');
    }
}
```

### Form widget properties

Form widgets can have properties that define their behavior and appearance that can be set in the form definition file. Define the configurable properties directly on the class. Then, within the `initialize` method, use the `fillFromConfig` method to  populate these properties with values from your form definition file. Here is an example of a form widget class with properties:

```php
namespace Author\Extension\FormWidgets;

class MyFormWidget extends \Igniter\Admin\Classes\BaseFormWidget
{
    //
    // Configurable properties
    //
    
    public string $mode = 'grid';
    
    public string $property = 'value';

    //
    // Object properties
    //

    public string $defaultAlias = 'myformwidget';

    public function initialize()
    {
        $this->fillFromConfig([
            'mode'
            'property'
        ]);
    }
}
```

You can then set these properties in the form definition file:

```php
'field' => [
    'label' => 'Field',
    'type' => 'myformwidget',
    'mode' => 'inline',
    'property' => 'new value',
],
```

### Form widget registration

To register a form widget, you must add it to the `registerFormWidgets` method in the extension class. The method returns an array containing the widget class in the keys and widget short code as the value. Here is an example of how to register a form widget:

```php
public function registerFormWidgets()
{
    return [
        'Author\Extension\FormWidgets\MyFormWidget' => 'myformwidget',
    ];
}
```

> **Note:** The widget short code should be a unique value.

### Loading form data

The primary function of a form widget is to interact with your model, by loading and saving data to the database. When a form widget is rendered, it retrieves its stored value using the `getLoadValue` method. Additionally, the `getId` and `getFieldName` methods provide a unique identifier and the name for an HTML element used in the form. These values are typically passed to the widget partial at the time of rendering.

Here's how you might implement this in the `render` method:

```php
public function render()
{
    $this->vars['id'] = $this->getId();
    $this->vars['name'] = $this->getFieldName();
    $this->vars['value'] = $this->getLoadValue();

    return $this->makePartial('myformwidget');
}
```

In the partial `myformwidget`, the HTML element can be rendered using the prepared variables:

```blade
<input 
    id="{{ $id }}
    name="{{ $name }}"
    value="{{ $value }}"
/>
```

### Saving form data

When it comes to storing user input in the database, the form widget uses the `getSaveValue` method internally to capture the value. If you need to modify this behavior, you can override this method in your form widget class:

```php
public function getSaveValue($value)
{
    return $value;
}
```

In scenarios where you might not want any value to be saved (e.g., a form widget that displays information without saving it), you can return a special constant `FormField::NO_SAVE_DATA` from the `Igniter\Admin\Classes\FormField` class to ensure the value is ignored:

```php
public function getSaveValue($value)
{
    return \Igniter\Admin\Classes\FormField::NO_SAVE_DATA;
}
```

## Using dashboard widget

Dashboard widgets in TastyIgniter are used to display information on the admin dashboard. They are similar to form widgets but are designed to be used on the dashboard page. Dashboard widgets can be registered in the [Extension class](../extend/extensions#extension-class).

Dashboard widget classes are located in the `src/DashboardWidgets` directory of an extension. Similarly to all form widgets, dashboard widgets can also include assets and partials. Here's an example:

```yaml
author/
  extension/
    src/
      DashboardWidgets/
        MyDashboardWidget.php
    resources/
      css/
        mydashboardwidget.css
      js/
        mydashboardwidget.js
      views/
        _partials/
          dashboardwidgets/
            mydashboardwidget.blade.php
```

### Dashboard widget class

The dashboard widget class is a PHP class that extends the `\Igniter\Admin\Classes\BaseDashboardWidget` class. Here is an example of a simple dashboard widget class:

```php
namespace Author\Extension\DashboardWidgets;

class MyDashboard extends \Igniter\Admin\Classes\BaseDashboardWidget
{
    public string $defaultAlias = 'mydashboardwidget';

    public function render()
    {
        $this->vars['var'] = 'value';
        
        return $this->makePartial('mydashboardwidget');
    }
}
```

When designing a widget partial, you can include various types of content such as charts, indicators, lists, or any other HTML markup. To ensure consistent styling and integration within the admin, wrap your markup within a **DIV** element with the class `dashboard-widget`. It's also recommended to use an **H3** element for the widget's header.

```blade
<div class="dashboard-widget">
    <h3>My Dashboard Widget</h3>
    
    <div>
        <ul>
            <li>First Data Point: <strong>100</strong></li>
            <li>Second Data Point: <strong>200</strong></li>
            <li>Third Data Point: <strong>300</strong></li>
        </ul>
    </div>
</div>
```

### Dashboard widget properties

Dashboard widgets can have properties that define their behavior and appearance. These properties can be defined using the `defineProperties` method in the dashboard widget class. Here is an example of a dashboard widget class with properties:

```php
namespace Author\Extension\DashboardWidgets;

class MyDashboard extends \Igniter\Admin\Classes\BaseDashboardWidget

    public function defineProperties(): array
    {
        return [
            'property' => [
                'label' => 'Property',
                'type' => 'text',
                'default' => 'value',
            ],
        ];
    }
}
```

### Dashboard widget registration

To register a dashboard widget, you must add it to the `registerDashboardWidgets` method in the extension class. The method returns an array containing the widget class in the keys and widget configuration (label, context, and required permissions) as the value. Here is an example of how to register a dashboard widget:

```php
public function registerDashboardWidgets()
{
    return [
        \Author\Extension\DashboardWidgets\MyDashboard::class => [
            'label' => 'My Dashboard Widget',
            'context' => 'dashboard',
            'permissions' => ['Author.Extension.*'],
        ],
    ];
}
```

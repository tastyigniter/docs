---
title: "Components"
section: extend
sortOrder: 140
---

## Introduction

Components implements content and features that extend your TastyIgniter website. They can be added, removed, and rearranged from _Design > Themes > Editor_ in the admin area.

Other than displaying HTML markup on a page, components can handle [AJAX requests](../advanced/ajax-request).

In this section, you'll learn the basics of building, registering and rendering both [Igniter](../extend/components#theme-component) and [Livewire](#livewire-component) components within a TastyIgniter application. Starting with version 4, you can use Livewire components within your theme layouts or pages. You can register both Igniter and Livewire components using the `registerComponents` method and attach them into your pages or layouts via the Admin Interface.

## Livewire Component

To create a Livewire component, extend the `\Livewire\Component` class, implement the `DefineComponent` interface, and register it to be displayed on your site and available from the Admin Interface. If you're new to Livewire component development, the [Components section of the Livewire documentation](https://livewire.laravel.com/docs/components) provides a good starting point.

Livewire components are stored in the `/src/Livewire` subdirectory within an extension directory. Additionally, component partials should be placed in `resources/views/livewire` to align with Laravel package conventions.

```yaml
vendor/
  acme/
    helloworld/                       <=== Extension directory
      src/                            <=== Extension source directory
        Livewire/                     <=== Livewire component subdirectory
          HelloBlock.php              <=== Component class file
      resources/                      <=== Extension resources directory
        views/                        <=== Extension views directory
          livewire/                   <=== Livewire blade views subdirectory
            hello-block.blade.php     <=== Component default blade view (optional)
```

### Defining the component

To begin, you can either use your preferred file manager to create files and directory for the **HelloBlock** component.

The component class file should extend the `\Livewire\Component` base class, add `\Igniter\Main\Traits\ConfigurableComponent` trait and define method `componentMeta`. The following is an example defines the `acme.hello-world::hello-block` component:

```php
namespace Acme\HelloWorld\Livewire;

class HelloBlock extends \Livewire\Component
{
    use \Igniter\Main\Traits\ConfigurableComponent;

    public int $maxItems = 5;
    
    public function componentMeta(): array
    {
        return [
            'code' => 'acme.hello-world::hello-block',
            'name' => 'Name of the hello block component',
            'description' => 'Description of the hello block component',
        ];
    }
    
    public function render()
    {
        return view('acme.helloworld::livewire.hello-block');
    }

    public function alerts(): array
    {
        return ['Success Alert', 'Warning Alert', 'Danger Alert'];
    }
}
```

The `componentMeta` method should return an array with the component code, name, and description. The `code` key is used to reference the component within blade views. The `name` and `description` keys are used to describe the component in the Admin Interface. This method is required for the component to be registered and available in the admin area.

The component properties and methods will automatically be made available to the component's view. For example, you will be able to access its `alerts` method and `$maxItems` property from the `resources/views/livewire/hello-block.blade.php` blade view. For example:

```blade
<p>Max alerts: {{ $maxItems }}</p>
@foreach ($this->alerts() as $message)
    <p>{{ $message }}</p>
@endforeach
```

### Component registration

After defining the component with the `componentMeta` method implemented, you can register the component by overriding the `registerComponents` method within the [Extension registration class](../extend/extensions#extension-class). This lets the app know about the component and so it can be attached and properties managed through the Admin Interface. To register the `acme.hello-world::hello-block` component, you may override the `registerComponents` method of the extension class:

```php
public function registerComponents(): array
{
    return [
        \Acme\HelloWorld\Livewire\HelloBlock::class,
    ];
}
```

### Component properties

Components can be configured using class properties. Let's define a **maxItems** property to limit the number of alerts allowed.

```php
namespace Acme\HelloWorld\Livewire;
 
class HelloBlock extends \Livewire\Component
{
    use \Igniter\Main\Traits\ConfigurableComponent;

    public int $maxItems = 5;
 
    // ...
}
```

To manage your component's properties through the admin area, you must implement the `defineProperties` method. The method should return an array with the property keys as indexes and the property configuration as values. The property keys must match the class property names.

```php
public function defineProperties(): array
{
    return [
        'maxItems' => [
            'label' => 'Max items',
            'type' => 'number',
        ]
    ];
}
```

| Key             | Description                                                                                                                                                                                                            |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **label**       | required, the property label, it is used by the component Selector in the Admin Interface.                                                                                                                             |
| **type**        | optional, specifies the property type. The type defines the form field type. Currently supported types are **text**, **number**, **checkbox**, **radio**, **select** and **selectlist**. Default value: **text**. |
| **default**     | optional, the default property value.                                                                                                                                                                                  |
| **comment**     | optional, the property description, it is used by the component Selector in the Admin Interface.                                                                                                                       |
| **placeholder** | optional placeholder for text and select properties.                                                                                                                                                                   |
| **options**     | optional array of options for checkbox, radio, select, selectlist properties.                                                                                                                                          |

The **options** property key can be static or dynamic. Using the `maxItems` property, let's define static options:

```php
public function defineProperties(): array
{
    return [
        'maxItems' => [
            'label' => 'Max items',
            'type' => 'select',
            'options' => [
                20 => '20 Per page', 
                50 => '50 Per page'
            ]
        ]
    ];
}
```

Dynamically, the options can be fetched by omitting the `options` key from the property definition and defining a static method that returns the list of options. The method name should be the studly cased name of the property. For example, `getPropertyOptions` where **Property** is the name of the property. In this example, we'll define a method for the `maxItems` property.

```php
public static function getMaxItemsOptions(): array
{
    return [
        20 => '20 Per page', 
        50 => '50 Per page'
    ];
}
```

#### Accessing component properties

Reading the property value from within the **component class**:

```php
$maxItems = $this->maxItems;
```

Accessing the property value from a **component blade view**:

```php
{{ $maxItems }}
```

For more information, reference the [Livewire Component Properties documentation](https://livewire.laravel.com/docs/properties).

### Component actions

Livewire actions are defined as class methods prefixed with `on` followed by the event name. These methods can be triggered by frontend activities such as clicking a button or filling out a form using the `wire:click` or `wire:submit` directives. For example, to handle an AJAX request with the event name `onAddItem`, define a method in the component class as follows:

```php
public function onAddItem()
{
    $value1 = post('value1');
    $value2 = post('value2');
    $this->page['result'] = $value1 + $value2;
}
```

The action can be triggered from the component's blade view as follows:

```blade
<button wire:click="onAddItem">Add Items</button>
```

For more information, reference the [Livewire Component Actions documentation](https://livewire.laravel.com/docs/actions).

### Component lifecycle handlers

Livewire includes a number of lifecycle hooks that allow you to run code at specific stages of a component's lifecycle. These hooks let you conduct activities before or after specific events, including as component initialization, property updates, and template rendering.

| Hook Method   | Description                                                                     |
|---------------|---------------------------------------------------------------------------------|
| **mount()**   | Called when a component is created                                              |
| **hydrate**   | Called when a component is re-hydrated at the beginning of a subsequent request |
| **boot**      | Called at the beginning of every request. Both initial, and subsequent          |
| **updating**  | Called before updating a component property                                     |
| **updated**   | Called after updating a property                                                |
| **rendering** | Called before render() is called                                                |
| **rendered**  | Called after render() is called                                                 |
| **dehydrate** | Called at the end of every component request                                    |

For more information, reference the [Livewire Component Lifecycle Hooks documentation](https://livewire.laravel.com/docs/lifecycle-hooks).

### Rendering the component

Use the admin interface to display components on pages and layouts. You can also display a component on a page or layout manually using a file editor by following the example below:

```blade
<livewire:acme.hello-world::hello-block />
```

The **acme.hello-world** prefix in the above example presents the code of the extension that registers the component.

#### Passing data into components

To pass data to a Livewire component rendered on a page or layout, you can define the data within the front-matter section of the page or layout template file. Here is an example passing the `maxItems` property to the `acme.helloworld::hello-block` component within a page:

```blade
---
title: My first page
permalink: "/page"

'[acme.helloworld::hello-block]':
  maxItems: 20
---

<p>Page content</p>

<livewire:acme.hello-world::hello-block />
```

> Keep the indentation consistent when defining component properties in the front-matter section.

You can pass data to components by defining them as attributes on the component tag. For example:

```blade
<livewire:acme.hello-world::hello-block maxItems="20" />
```

If you need to pass dynamic values or variables to a component, you can write PHP expressions in component attributes by prefixing the attribute with a colon:

```blade
<livewire:acme.hello-world::hello-block :maxItems="$initialCount" />
```

You can access variables within the component like any other markup variable:

```blade
<ul>
    @foreach($pages as $page)
        <li>{{ $page->name }}</li>
    @endforeach
</ul>
```

For more information, reference the [Rendering components section of the Livewire documentation](https://livewire.laravel.com/docs/components#rendering-components).

#### Rendering multiple instances of the same component

To render multiple instances of the same component on a page or layout, you can assign each instance a unique alias. For example:

```blade
<livewire:acme.hello-world::hello-block />
<livewire:acme.hello-world::hello-block :alias="unique-key" />
```

The `:alias` property behaves different from [Livewire's wire:key attribute](https://livewire.laravel.com/docs/components#adding-wirekey-to-foreach-loops) and should not be used as a substitute.

You can pass data to each instance of the component by defining the data within the front-matter section of the page or layout template file. For example:

```yaml
---
title: My first page
permalink: "/page"

'[acme.helloworld::hello-block]':
  maxItems: 20

'[acme.helloworld::hello-block unique-key]':
  maxItems: 5
---
```

### Inject page assets

Use the `Assets::addCss` and `Assets::addJs` methods to add assets to the pages the components are attached to:

```php
public function mount()
{
    Assets::addJs('acme.helloworld::/js/block.js', 'helloworld-block');
    Assets::addCss('acme.helloworld::/js/block.css', 'helloworld-block');
}
```

## Theme Component

To create a theme component, extend the `BaseComponent` class, implement its methods, and register it to be displayed on your site and available from the Admin Interface.

Theme component classes live in the **/src/Components** subdirectory of an extension directory, while the component partials live in the **/resources/views/_components** subdirectory.

```yaml
vendor/
  acme/
    helloworld/               <=== Extension directory
      src/                    <=== Extension source directory
        Components/           <=== Components subdirectory
          HelloBlock.php      <=== Component class file
      resources/              <=== Extension resources directory
        views/                <=== Extension views directory
          _components/        <=== Components subdirectory
            helloBlock/       <=== Component subdirectory
              default.blade.php <=== Component default partial (optional)
```

### Defining the component

To begin, you can either use your preferred file manager to create files and directory for the **HelloBlock** component or simply call the following command from the application directory to generate a component with basic files and directories:

```bash
php artisan make:igniter-component Acme.HelloWorld HelloBlock
```

The component class file should extend the `\Igniter\System\Classes\BaseExtension` base class and define the functionality and properties of the component. The following is an example defines the `HelloBlock` component:

```php
namespace Acme\HelloWorld\Components;

class HelloBlock extends \Igniter\System\Classes\BaseComponent
{
    public function defineProperties(): array
    {
        return [];
    }
    
    public function alerts(): array
    {
        return ['Success Alert', 'Warning Alert', 'Danger Alert'];
    }
}
```

The component properties and methods are available on the page or layout it is attached to through the component variable
which corresponds to the alias of the component. For example, if the `HelloBlock` component was defined on a page or
layout as `'[helloBlock]'`, you will be able to access its `alerts` through the `$helloBlock` variable:

```blade
@foreach ($helloBlock->alerts() as $message)
    <p>{{ $message }}</p>
@endforeach
```

### Component registration

Components must be registered by overriding the `registerComponents` method within the [Extension registration class](../extend/extensions#extension-class). This lets the app know about the component and gives it an **alias name** for its use within themes. To register the `HelloBlock` component with the default alias name **helloBlock**, you may override the `registerComponents` method:

```php
public function registerComponents(): array
{
    return [
        \Acme\HelloWorld\Components\HelloBlock::class => [
            'code' => 'helloBlock',
            'name' => 'Name of the hello block component',
            'description' => 'Description of the hello block component',
        ],
    ];
}
```

### Component properties

Components can be configured using properties defined for each component by overriding the `defineProperties` method. Let's define a **maxItems** property to limit the number of alerts allowed

```php
public function defineProperties(): array
{
    return [
        'maxItems' => [
             'title' => 'Max items',
             'description' => 'The most number of alert items allowed',
             'default' => 10,
             'type' => 'text',
        ]
    ];
}
```

The method should return an array with the property keys as indexes and the property parameters as values. The property keys are used to access the component property values within the component class.

| Key             | Description                                                                                                                                                                                                            |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **label**       | required, the property label, it is used by the component Selector in the Admin Interface.                                                                                                                             |
| **type**        | optional, specifies the property type. The type defines the form field type. Currently supported types are **text**, **number**, **checkbox**, **radio**, **select** and **selectlist**. Default value: **text**. |
| **default**     | optional, the default property value.                                                                                                                                                                                  |
| **comment**     | optional, the property description, it is used by the component Selector in the Admin Interface.                                                                                                                       |
| **placeholder** | optional placeholder for text and select properties.                                                                                                                                                                   |
| **options**     | optional array of options for checkbox, radio, select, selectlist properties.                                                                                                                                          |

The **options** property key can be static or dynamic. Using the `maxItems` property, let's define static options:

```php
public function defineProperties(): array
{
    return [
        'maxItems' => [
            ...
            'type' => 'select',
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
public function defineProperties(): array
{
    return [
        'maxItems' => [
            ...
            'type' => 'select',
        ]
    ];
}

public static function getMaxItemsOptions(): array
{
    return [
        '20' => '20 Per page', 
        '50' => '50 Per page'
    ];
}
```

#### Accessing component properties

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

### Component class methods

Sometimes you might want to handle some initialization logic before AJAX handlers and before the execution cycle of the page. You can override the `initialize` method in the component class to handle such initialization logic.

The following methods are supported in the component class:

| Method              | Description                                                                              |
|---------------------|------------------------------------------------------------------------------------------|
| **initialize()**    | initialization method, called when the component is attached to a page or layout         |
| **onRun()**         | override to hook into the page's execution life cycle.                                   |
| **onRender()**      | override to handle specific logic before rendering the default partial of the component. |
| **renderPartial()** | render a specified component partial or shared partial.                                  |

### Component AJAX handlers

Components class can define AJAX event handlers as methods prefixed with `on` followed by the event name. For example, to handle an AJAX request with the event name `onAddItem`, define a method in the component class as follows:

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

### Component partials

Besides the default partial, components can also provide additional partials which can be used within other template files or in the default partial itself.

Components can share partial files by placing them in the **resources/views/_partials** subdirectory of the extension directory.

```yaml
vendor/
  acme/
    helloworld/
      resources/
        views/
          _components/    <=== Components directory
            helloBlock/    <=== Component partials directory
              default.blade.php     <=== Component default partial file
          _partials/    <=== Component shared partials directory
            shared.blade.php     <=== Component shared partial file
```

Blade's `@themePartial` directive allows you to include the **helloBlock** component partial from within another component partial:

```blade
@themePartial('helloBlock::shared')
```

Partials rendered from outside the component must use their fully qualified name.

```blade
@themePartial('componentName::component-partial')
```

#### Overriding component partials

All component partials can be overridden by creating theme partials. A component defined with alias **helloBlock** can have its **default.blade.php** partial overridden by creating a theme partial called **_partials/helloBlock/default.blade.php**

### Component lifecycle handlers

Every time the page loads, the `MainController` executes the `onRun` method of the attached components. Overriding the method in the component class allows components to hook into the page execution cycle. Let's inject some variables into the page:

```php
public function onRun()
{
    $this->page['var'] = 'value';
}
```

Handler functions can be defined in the layout and page [PHP code section](../customize/themes#php-code-section) and component classes. Here is an example of a page or layout PHP section with handler functions:

```blade
---
public function onStart()
{
    //
}
---
<p>HTML markup</p>
```

These handlers are executed in the following sequence:

- `onInit` layout function
- `onInit` page function
- `onStart` layout function
- `onRun` layout components function
- `onStart` page function
- `onRun` page components function
- `onEnd` page function
- `onEnd` layout function

### Rendering the component

Use the admin interface to attach components to pages and layouts. You can also attach a component to a page or layout manually using a file editor by following the example below:

```yaml
---
title: My first page
permalink: "/page"

'[helloBlock]':
  maxItems: 20
---
```

The **helloBlock** component in the above example initializes the property **maxItems** with value **20**.

Render components HTML markup on a page or layout as follows:

```blade
@themeComponent('helloBlock')
```

#### Component variables

When you attach a component, a page variable that matches the component name is automatically created (`$helloBlock` in the previous example).

The `@themeComponent('helloBlock')` tag accepts an array of variables as its second parameter. The specified variables will be available at the time of rendering and will explicitly override the component property values:

```blade
@themeComponent('helloBlock', ['maxItems' => 100])
```

#### Rendering multiple instances of the same component

If two components with the same name are assigned to a page and layout together, the page component overrides any properties of the layout component.

You can render multiple instances of the same components by assigning it an _alias_:

```blade
title: My first page
permalink: "/page"

'[helloBlock helloBlockA]:
  maxItems: 1

'[helloBlock helloBlockB]:
  maxItems: 5
---
@themeComponent('helloBlockA')

@themeComponent('helloBlockB')
```

> Keep the indentation consistent when defining component properties in the front-matter section.

### Inject page assets

Use the `addCss` and `addJs` controller methods to add assets to the pages or layouts the components are attached to:

```php
public function onRun()
{
    // Assets path that begins with a slash (/) is relative to the website root directory
    $this->addJs('acme.helloworld::/js/block.js', 'helloworld-block');

    // Paths that does not begin with a slash is relative to the component directory
    $this->addJs('js/block.js', 'helloworld-block');
}
```

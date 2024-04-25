---
title: "Markup Guide"
section: customize
sortOrder: 150
---

## Introduction

TastyIgniter uses the [Blade template engine](https://laravel.com/docs/blade) to render views. Blade is a simple, yet powerful templating engine provided with Laravel. Unlike other popular PHP templating engines, Blade does not restrict you from using plain PHP code in your views. All Blade views are compiled into plain PHP code and cached until they are modified, meaning Blade adds essentially zero overhead to your application.

Blade views are typically stored in the `resources/views` directory of your theme or extension. The Blade file extension is `.blade.php`.

## Variables

The following variables are available in all layout and page templates:

- `$this->page` - The current page object.
- `$this->layout` - The current layout object.
- `$this->theme` - The current theme object.
- `$this->param` - The current request parameters.

Use these variables to access the properties of the current page, layout, theme, or request parameters. For example, to access the title of the current page, use `$this->page->title`.

You may display variables in your templates using the `{{ $variable }}` syntax. For example, to display the title of the current page, use `{{ $this->page->title }}`.

## Directives

Directives are a unique feature to Laravel Blade and are prefixed with `@` character. TastyIgniter extends the [Blade template engine](https://laravel.com/docs/blade) by adding a number of directives and variables.

> **Note:** The Blade directives provided by TastyIgniter are not the same as the Laravel Blade directives. The Blade directives provided by TastyIgniter are specific to TastyIgniter and are not compatible with Laravel.

### @themePage

The `@themePage` tag renders the contents of a page into a layout template. The directive has no parameters.

```blade
@themePage
```

### @themePartial

The `@themePartial` directive renders a partial template file located under the `_partials` subdirectory of a theme. If you are rendering blade views from the other subdirectory of a theme, use Blade's `@include` directive instead.

The directive has a single required parameter - the name of the partial file without the `.blade.php` extension. You can specify the name of the subdirectory if you refer a partial from a subdirectory `@partial('directory.partial-name')`.

```blade
@themePartial('partial-name')
```

The directive also accepts a second parameter - an optional array of data to pass to the partial. For example:

```blade
@themePartial('partial-name', ['status' => 'complete'])
```

If you need to render a partial only if it exists, you can use the `@themePartialIf` directives.

```blade
@themePartialIf('partial-name', ['status' => 'complete'])
```

If you need to render a partial only if a boolean condition evaluates to `true` or `false`, you can use the `@themePartialWhen` and `@themePartialUnless` directives.

```blade
@themePartialWhen($expression, 'partial-name', ['status' => 'complete'])

@themePartialUnless($expression, 'partial-name', ['status' => 'complete'])
```

If you need to render the first partial that exists from a given array of partials, you can use the `@themePartialFirst` directive.

```blade
@themePartialFirst(['custom.partial-name', 'partial-name'], ['status' => 'complete'])
```

### @themeComponent

The `@themeComponent` directive renders a Theme component. The directive has a single required parameter - the name of the component. To render a Livewire component, use the `@livewire` directive instead.

```blade
@themeComponent('componentName')
```

The directive also accepts a second parameter - an optional array of data to pass to the component. For example:

```blade
@themeComponent('componentName', ['status' => 'complete'])
```

If you need to render a component only if it exists, you can use the `@themeComponentIf` directives.

```blade
@themeComponentIf('componentName', ['status' => 'complete'])
```

If you need to render a component only if a boolean condition evaluates to `true` or `false`, you can use the `@themeComponentWhen` and `@themeComponentUnless` directives.

```blade
@themeComponentWhen($expression, 'componentName', ['status' => 'complete'])

@themeComponentUnless($expression, 'componentName', ['status' => 'complete'])
```

If you need to render the first component that exists from a given array of components, you can use the `@themeComponentFirst` directive.

```blade
@themeComponentFirst(['componentNameExtended', 'componentName'], ['status' => 'complete'])
```

### @themeContent

The `@themeContent` directive renders the contents of a static template file located under the `_content` subdirectory of a theme. If you are rendering blade views from the other subdirectory of a theme, use Blade's `@include` directive instead.

The directive has a single required parameter - the name of the content template file without the `.htm` extension. You can specify the name of the subdirectory if you refer a content from a subdirectory `@themeContent('directory.content-name')`.


```blade
@themeContent('content-name')
```

### @themeStyles

The `@themeStyles` directive renders all stylesheets that are defined during the rendering of the page. This includes all `.css` files registered within the `resources/meta/assets.json` manifest file, as well as stylesheets injected using the controller's `addCss` method. The directive has no parameters.

```blade
<head>
    ...
    @themeStyles
</head>
<body>
    ...
</body>
```

### @themeScripts

The `@themeScripts` directive renders all scripts that are defined during the rendering of the page. This includes all `.js` files registered within the `resources/meta/assets.json` manifest file, as well as scripts injected using the controller's `addJs` method. The directive has no parameters.

```blade
<head>
    ...
</head>
<body>
    ...
    @themeScripts
</body>
```

## Blade directives

Blade also provides a variety of directives for working with control structures, such as loops and conditional statements.

### If statements

You can construct if statements using the Blade directives `@if`, `@elseif`, `@else`, and `@endif`.

```blade
@if (count($categories) === 1)
    I have one category!
@elseif (count($categories) > 1)
    I have multiple categories!
@else
    I don't have any categories!
@endif

@unless (Cart::content())
    Cart is empty.
@endunless

@isset($categories)
    // $categories is defined and is not null...
@endisset
 
@empty($categories)
    // $categories is "empty"...
@endempty
```

### Switch statements

You can construct switch statements using the Blade directives `@switch`, `@case`, `@break`, `@default`, and `@endswitch`.

```blade
@switch($i)
    @case(1)
        First case...
        @break

    @case(2)
        Second case...
        @break

    @default
        Default case...
@endswitch
```

### Loops

You can construct loops using the Blade directives `@for`, `@foreach`, `@forelse`, `@while`, and `@endfor`, `@endforeach`, `@endforelse`, `@endwhile`.

```blade
@for ($i = 0; $i < 10; $i++)
    The current value is {{ '{{ $i }}' }}
@endfor

@foreach ($categories as $category)
    <p>This is category {{ '{{ $category->name }}' }}</p>
@endforeach

@forelse ($categories as $category)
    <li>{{ '{{ $category->name }}' }}</li>
@empty
    <p>No categories</p>
@endforelse

@while (true)
    <p>I'm looping forever.</p>
@endwhile
```

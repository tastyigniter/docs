---
title: "Layouts"
section: customize
sortOrder: 120
---

## Introduction

In TastyIgniter, a theme layout is a template that wraps around your content. It allows you to have your template source code in one place so that you don't have to repeat things like your header and footer on every page.

Layout files live in the **resources/views/_layouts** subdirectory of a theme directory. For example:

```yaml
acme/                           <=== Theme vendor directory
  purple/                       <=== Theme directory
    resources/
      views/
        _layouts/              <=== Layouts subdirectory
          default.blade.php  <=== Layout template file
```

The convention is to have a basic layout called `default.blade.php` and be used by other pages as required. Within the layout file, you should use the `@themePage`  tag to display the content of the page.

```blade
<!DOCTYPE html>
<html>
    <head>
        <!-- -->
    </head>
    <body>
        @themePage
    </body>
</html>
```

## Front matter section

The front matter section is optional for layouts. The optional front matter parameter `description` is used in the Admin Interface.

| Variable       | Description                                                   |
|----------------|---------------------------------------------------------------|
| `description`  | Use this variable as the layout title or description or both. |

Here is an example of a layout file:

```blade
---
description: My first layout example
---
<!DOCTYPE html>
<html>
    <head>
        <!-- -->
    </head>
    <body>
        @themePage
    </body>
</html>
```

You can set the front matter in layouts, the only difference is you use the `$this->layout` object instead of the `$this->page`. For example:

```blade
---
description: My first layout example
---
<p>{{ $this->layout->description }}</p>

@themePage
```

You can pass your own variables to the layout by defining them in the front matter section. For example:

```blade
---
description: My first layout example
food: "Pizza"
---
<p>{{ $this->layout->food }}</p>
```

## PHP code section

The PHP code section and front matter are both optional, but the HTML markup section is required. For example:

```blade
---
description: My first layout example
---
<?php
function onStart()
{
    $this['hello'] = "Hello world!";
}
?>
---
<h3>{{ $this->layout->hello }}</h3>
```

## Using layouts

To use a layout, you need to specify the layout file in the front matter of your page. For example, if you have a layout file called `default.blade.php`, you would specify `default` as the layout in the front matter of your page. For example:

```blade
---
permalink: "/page"
layout: default
---
<p>This is the content of my page</p>
```

## Including partials

[Partials](../customize/partials) are reusable Blade markup blocks that can be used in layouts, pages and other partials. Here, we'll cover the basics of rendering partials within a layout.

You can render a partial in a layout using Blade's `@include` directive. For example:

```blade
<!DOCTYPE html>
<html>
    <head>
        <!-- -->
    </head>
    <body>
        @include('acme.purple::includes.header')
        
        @themePage
    </body>
</html>
```

> `acme.purple` is the theme code specified in the [theme manifest file](../customize/themes#theme-manifest) and `includes.header` is the partial located in `resources/views/includes/header.blade.php`.

You can pass variables to partials by defining them in the `@include` directive after the partial name. For example:

```blade
@include('acme.purple::includes.header', ['pages' => $pages])
```

You can access variables within the partial like any other template variable:

```blade
<ul>
    @foreach($pages as $page)
        <li>{{ $page->name }}</li>
    @endforeach
</ul>
```

## Rendering components

[Components](../extend/components) are reusable Blade markup blocks combining state and behavior that serve as building blocks in layouts or pages. Here, we'll cover the basics of rendering [Livewire components](../extend/components#livewire-component) within a layout.

Livewire component consists of two files. The first is the component class `src/Livewire/HelloBlock.php` and the second is the component template file `resources/views/livewire/hello-block.blade.php`.

You can render a Livewire component in a layout using the `<livewire:extension-or-theme-code::component-name />` syntax. For example:

```blade
<!DOCTYPE html>
<html>
    <head>
        <!-- -->
    </head>
    <body>
        @themePage

        <livewire:acme.purple::hello-block />
    </body>
</html>
```

For more on components, see the [Components](../extend/components) documentation.

## Execution life cycle

Specific functions can be defined in the layouts PHP code section for handling the page execution life cycle: `onInit`, `onStart` and `onEnd`.

The `onInit` function is executed when all [components](../extend/components) are initialized and before AJAX requests are handled. The `onStart` function is executed at the start of the execution of the page. The `onEnd` function executes after the page renders.

```blade
---
name: My First Layout
description: My first layout example
---
<?php
function onStart()
{
    $this['hello'] = "Hello world!";
}
?>
---
<h3>{{ $this->layout->hello }}</h3>
```

In this example, the `onStart` function is executed at the start of the execution of the page and sets a variable `hello` that is then used in the layout and can be used in the page content.

## Inject page assets

The controller's `addCss` and `addJs` methods allow you to inject assets files (CSS and JavaScript) to layouts. This can be done in the `onStart` function defined in a layout template PHP section.

```php
---
<?php
function onStart()
{
    $this->addCss('assets/css/my-style.css');
    $this->addJs('assets/js/my-script.js');
}
?>
---
```

In order to output the injected assets on pages and layouts use the `@themeStyles` and `@themeScripts` tags. Example:

```blade
<head>
    ...
    @themeStyles
</head>
<body>
    ...
    @themeScripts
</body>
```

> The page output in the above example will also include all assets files registered within the `resources/meta/assets.json` manifest file.

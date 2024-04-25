---
title: "Pages"
section: customize
sortOrder: 110
---

## Introduction

All websites are made up of pages. In TastyIgniter, pages are the most basic building block for content. Page file names or directory structure do not affect the routing, but naming your pages according to the page function is a good idea.

Pages files live in the **resources/views/_pages** subdirectory of a theme directory. For example, a page file named `home.blade.php` located in the **_pages** directory of a theme.

```yaml
acme/                           <=== Theme vendor directory
  purple/                       <=== Theme directory
    resources/
      views/
        _pages/         	    <=== Pages subdirectory
          home.blade.php		<=== Page template file
```

A page file consists of three sections: front matter, PHP code (optional), and HTML markup. Here's an example of a page file:

```blade
---
title: My Home Page
description: "My home page example"
permalink: "/"
layout: default
---
<?php
function onStart()
{
    $this['hello'] = "Hello world!";
}
?>
---
<p>{{ $hello }}, this is the content of my page</p>
```

## Front matter section

The front matter section is **required** for pages. A number of predefined parameters can be set in the front-matter of a page.

| Variable      | Description                                                                                                    |
|---------------|----------------------------------------------------------------------------------------------------------------|
| `title`       | Use this variable as the page title.                                                                           |
| `description` | Use this variable as the page description.                                                                     |
| `permalink`   | Set this variable and use it as the page URL, **required**.                                                    |
| `layout`      | If set, this specifies the layout file to be used. Use the name of the layout file without the file extension. |

You can also define your own front-matter variables that can be accessed in PHP. For example, if you set a `food` variable, you can use it on your page:

```blade
---
food: "Pizza"
---
<h1>{{ $this->page->food }}</h1>
```

### Permalink variable

The `permalink` variable is used to set the URL of the page. It can contain parameters and can be set using front matter. Permalinks should start with the forward slash character `/`.

For example, you might have a page on your site located at `resources/views/_pages/common/about.blade.php` and you want the url to be `/about`. In front matter of the page you would set:

```blade
permalink: "/about"
```

### Permalink parameters

For any address such as `/pages/my-first-page`, a page with the **permalink pattern** defined in the following example would be displayed.

```blade
permalink: "/pages/:title" - this will match /pages/page-title
```

To make a parameter optional add the **question mark** after its name:

```blade
permalink: "/pages/:title?" - this will match /pages/page-title OR /pages
```

Parameters can not be optional in the **middle** of the permalink:

```blade
permalink: "/pages/:title?/:slug" - this will match /pages/page-title/page-slug
```

A default value can be specified after the **question mark** and used as fallback values in case the real parameter value is not presented. **Default values can not contain any asterisks, pipe symbols or question marks.**

```blade
permalink: "/pages/:title?default" - this will match /pages/page-title OR /pages/default
```

You could also use regular expressions to validate parameters:

```blade
permalink: "/pages/:title|^[a-z0-9\-]+$" - this will match /pages/page-title
```

A special wildcard parameter can be used by placing the asterisk after the parameter:

```blade
permalink: "/pages/:title*/:slug" - this will match /pages/page-title/child/page/page-slug
```

## PHP code section

The PHP code section is optional, but the front matter and the HTML markup sections are both required. For example:

```blade
---
permalink: "/"
---
<?php
function onStart()
{
    $this['hello'] = "Hello world!";
}
?>
---
<h3>{{ $this->page->hello }}</h3>
```

## Using layouts

To use a layout, you need to specify the layout file in the front matter of your page. For example, if you have a layout file called `default.blade.php`, you would specify `default` as the layout in the front matter of your page. For example:

```blade
---
permalink: "/"
layout: default
---
<p>This is the content of my page</p>
```

## Including partials

[Partials](../customize/partials) are reusable Blade markup blocks that can be used in pages, layouts and other partials. Here, we'll cover the basics of rendering partials within a page.

You can render a partial in a page using Blade's `@include` directive. For example:

```blade
---
permalink: "/"
food: "Pizza"
---
@include('acme.purple::includes.sidebar')

<p>{{ $this->page->food }}</p>
```

> `acme.purple` is the theme code specified in the [theme manifest file](../customize/themes#theme-manifest) and `includes.sidebar` is the partial located in `resources/views/includes/sidebar.blade.php`.

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

[Components](components.md) are reusable Blade markup blocks combining state and behavior that serve as building blocks in layouts or pages. Here, we'll cover the basics of rendering [Livewire components](components.md#livewire-component) within a page.

Livewire component consists of two files. The first is the component class `src/Livewire/HelloBlock.php` and the second is the component template file `resources/views/livewire/hello-block.blade.php`.

You can render a Livewire component in a page using the `<livewire:extension-or-theme-code::component-name />` syntax. For example:

```blade
---
permalink: "/"
food: "Pizza"
---
<livewire:acme.purple::hello-block />

<p>{{ $this->page->food }}</p>
```

For more on components, see the [Components](components.md) documentation.

## Execution life cycle

In the PHP code section of pages and layouts there are specific functions: `onInit`, `onStart` and `onEnd`.

The function `onInit` is executed when all [components](../customize/components) are initialized and before AJAX requests are handled. The `onStart` function is executed at the start of the execution of the page. The `onEnd` function is executed before the page renders and the page components executed.

```blade
---
<?php
function onEnd()
{
    $this['hello'] = "Hello world!";
}
?>
---
<h3>{{ $this->page->hello }}</h3>
```

## Custom response

Return a response from any of the methods defined in the execution life cycle. The life cycle methods have the ability to halt the process when a response is returned. Return `Hello world!` string to the browser without loading any page contents with the following example:

```php
---
<?php
public function onStart()
{
    return 'Hello world!';
}
?>
---
```

Below is an example of triggering a redirect using the `Redirect` facade:

```php
---
<?php
public function onStart()
{
    return Redirect::to('http://google.com');
}
?>
---
```

## Inject page assets

The controller's `addCss` and `addJs` methods allow you to inject assets files (CSS and JavaScript) to pages. This can be done in the `onStart` function defined in a page template PHP section.

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

In order to output the injected assets on pages or layouts use the `@themeStyles` and `@themeScripts` tags. Example:

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


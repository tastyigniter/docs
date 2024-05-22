---
title: "Themes"
section: customize
sortOrder: 100
---

## Introduction

TastyIgniter themes are files that work together to create a TastyIgniter website. Each theme can be different, offering site owners many choices to change their website look instantly. Themes, just like extensions are built on the foundation of Laravel packages.

Themes typically contains all the [pages](../customize/pages), [partials](../customize/partials), [layouts](../customize/layouts), assets files and an optional theme PHP file (`theme.php`). Additionally, a theme can have a manifest file (`theme.json`) and a meta directory (`_meta`) that contains the assets manifest file (`assets.json`) and a fields file (`fields.php`) for the Theme settings feature through the Admin Interface. 

Activating a theme can be done through the _Design > Themes_ Admin page with the Theme Selector or by running this command:

```bash
php artisan igniter:util set theme --theme=your-theme
```

This article is about developing themes for TastyIgniter.

## Directory structure

Below is an example of a theme directory structure. Every TastyIgniter theme is represented by way of a separate directory and typically a theme is activated to display the website.

```yaml
acme/                         <=== Theme vendor directory
  purple/                     <=== Theme directory
    public/
      css/                     <=== Directory contains compiled CSS files
      js/                      <=== Directory contains compiled JavaScript files
      images/                  <=== Directory contains image files
    resources/                 <=== Theme resources directory
      scss/                    <=== Directory contains source SCSS files
      js/                      <=== Directory contains source JavaScript files
      meta/                    <=== Meta directory
        assets.json            <=== Registers global css, js files and HTML meta tags
        fields.php             <=== Registers form fields for the theme customization
      views/                   <=== Theme views directory
        _layouts/              <=== Layouts directory
          default.blade.php
        _pages/                <=== Pages directory contains the website pages
          home.blade.php
        includes/             <=== Partials directory contains reusable HTML chucks
          sidebar.blade.php
    screenshot.png
    composer.json              <=== Manifest file
    theme.php                  <=== Theme PHP file - Loaded on every theme page request just before running the page code.
```

TastyIgniter supports a single level subdirectory for layouts, pages and partials files (any structure can be used in the `assets` directory), making it easier to organise large websites. 

A theme can contain any number of other subdirectories as well. 

**Screenshot**

Create a screenshot for your theme. The screenshot should be named `screenshot.png` and should be placed in the top level directory. The recommended image size is 1200px wide by 900px tall. The screenshot is usually smaller but the over-size image allows high-resolution viewing on HiDPI displays.

## Theme manifest

A `composer.json` file for a theme looks like this:

```json
{
  "name": "acme/ti-theme-purple",
  "type": "tastyigniter-package",
  "description": "Purple theme for TastyIgniter",
  "authors": [
    {
      "name": "Acme Labs"
    }
  ],
  "require": {
    "tastyigniter/ti-theme-orange": "*"
  },
  "extra": {
    "tastyigniter-theme": {
      "code": "acme-purple",
      "name": "Purple Theme",
      "locked": true,
      "source-path": "/resources/views",
      "meta-path": "/resources/meta",
      "publish-paths": [
        "/public"
      ]
    }
  }
}
```

| Field                               | Description                                                                                                                                                                                                                                                                                      |
|-------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **name**                            | The Composer package's name in vendor/package format, **required**. You should use a vendor name that is unique to you, such as your GitHub username. You should prefix the package part with <code><b>ti-theme-</b></code> to indicate that your package is intended for use with TastyIgniter. |
| **type**                            | MUST be set to <code><b>tastyigniter-package</b></code>, ensures that your theme will be installed as such when someone "requires" it, **required**.                                                                                                                                             |
| **description**                     | A one-sentence summary of what the extension does, **required**. (max. char: 130)                                                                                                                                                                                                                |
| **authors**                         | An object to specify the name of the extension author, **required**.                                                                                                                                                                                                                             |
| **authors.0.name**                  | An object to specify the name of the extension author, **required**.                                                                                                                                                                                                                             |
| **authors.0.email**                 | An object to specify the email of the extension author, **optional**.                                                                                                                                                                                                                            |
| **require**                         | Defines other TastyIgniter packages your theme depends on, **optional**. In the example above, **acme-purple** theme depends on the **tastyigniter/ti-theme-orange** theme.                                                                                                                      |
| **extra.tastyigniter-theme**        | Holds TastyIgniter-specific extension metadata, such as your theme's display name and paths, **required**.                                                                                                                                                                                       |
| **extra.tastyigniter-theme.code**   | the theme code, **required**. The value is used on the TastyIgniter marketplace for setting the theme code value.                                                                                                                                                                                |
| **extra.tastyigniter-theme.name**   | specifies the theme name, **required**.                                                                                                                                                                                                                                                          |
| **extra.tastyigniter-theme.locked** | specifies whether a child theme must be created to customize the theme, **optional**.                                                                                                                                                                                                            |

## Assets manifest

Before adding your files to the theme assets manifest file, you must place them within your theme in the correct directory structure, as shown in the [theme directory structure](#directory-structure).

A `resources/meta/assets.json` file looks like this:

```json
{
  "doctype": "html5",
  "favicon": "favicon.ico",
  "meta": [
    {
      "name": "Content-type",
      "content": "text/html; charset=utf-8",
      "type": "equiv"
    },
    {
      "name": "viewport",
      "content": "width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no",
      "type": "name"
    }
  ],
  "css": [
    {
      "path": "$/acme-purple/css/app.css",
      "name": "app-css"
    }
  ],
  "js": [
    {
      "path": "$/acme-purple/js/app.js",
      "name": "app-js"
    }
  ],
  "bundles": [
    {
      "type": "scss",
      "files": "acme-purple::/scss/app.scss",
      "destination": "$/acme-purple/css/app.css"
    }
  ]
}
```
> The `$` symbol is a placeholder for the `public` path. Theme assets are published to the `public` directory of the TastyIgniter installation when you install or update the theme or run the `php artisan igniter:theme-vendor-publish --theme=your-theme` command.

JavaScript code should be placed in external files whenever possible. Use [`@scripts`](../customize/markup-guide#themescripts) to load your scripts and [`@styles`](../customize/markup-guide#themestyles) to load your styles.

## Template structure

Template is a file that defines how a page or layout is organized and how its content is displayed. It can contain up to three sections: front-matter, PHP code, and HTML markup. These sections are separated by the `---` sequence.

A **page template** file looks like this:

```blade
---
title: Homepage
permalink: /
layout: default

'[helloBlock]':
    maxItems: 1
---
<?php
function onStart()
{
    //...
}
?>
---
<div>
    <p>Rendering a Theme component</p>
    @themeComponent('helloBlock')
    
    <p>Rendering a Livewire component</p>
    <livewire:hello-block />
    
    <p>Rendering sub views</p>
    @include('acme-purple::includes.sidebar')
</div>
```

A **layout template** file looks like this:

```blade
---
description: Default Layout
---
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{{ $this->page->title }}</title>
    @themeStyles
</head>
<body>
    @themePage
    @themeScripts
</body>
</html>
```

### Front-matter section

This is the first section in the file which contains valid YAML set between triple-dashed lines. You can set predefined variables or create custom ones which will be available to you to access from component blade views. Here is a basic example:

```blade
---
title: Homepage
permalink: /
layout: default

'[helloBlock]':
    maxItems: 1
---
```

### PHP code section

This section is executed each time before the template is rendered. It is optional and the content depends on the type of template it is defined within. The PHP open `<?php` and close `?>` tags should always be set
within the section separator `---` on a different line. For example:

```blade
---
<?php

function onStart() {
    $this['categories'] = ['Appetizer', 'Main Course', 'Dessert'];
}

?>
---
<h3>Categories</h3>
<ul>
    @foreach ($categories as $category) {
        <li>{{ $category }}</li>
    @endforeach
</ul>
```

The PHP section is converted to a PHP class when the template is parsed, you can only define functions and `use` namespaces, no other PHP code is allowed. You can access `$this` object to read and set variables.

### HTML markup

The HTML section defines the markup to be rendered by the template. The HTML section content can contain both HTML and PHP tags, functions, the content depends on whether the template is a page or layout.

## Including theme partials

To render the `includes/sidebar.blade.php` blade view from the [directory structure](../customize/themes#directory-structure) above within a page or layout or another partial, you can use `@include('includes.sidebar')` to make it easy for a theme to reuse code sections. The template paths are always absolute. If you render a partial from the subdirectory `includes/blog/`, the subdirectory name still needs to be specified, `@include('includes.blog.sidebar')`.

## Global variables

| Variable       | Description                                                                                                                     |
|----------------|---------------------------------------------------------------------------------------------------------------------------------|
| **theme**      | Theme object `$this->theme` for reading customized settings.                                                                    |
| **page**       | Page object `$this->page`. Custom variables set via front matter in [pages](../customize/pages) will be available here.         |
| **layout**     | Layout object `$this->layout`. Custom variables set via front matter in [layouts](../customize/layouts) will be available here. |
| **controller** | Access the underlying `MainController` object `$this->controller`.                                                              |

## Theme settings

The theme settings feature in TastyIgniter allows you to customize the appearance and behavior of your theme directly from the admin interface. This feature is available by default for enabled TastyIgniter themes from the _Design > Themes_ Admin page.

To enable theme settings, you need to register form fields using the `resources/meta/fields.php` file in your theme directory. This file should return an array of form fields that will be displayed in the theme settings interface. 

Here's an example of a `resources/meta/fields.php` file:

```php
return [
    // Set form fields for the admin theme settings.
    'form' => [
        'fields'  => [
            'font_family' => [
                'label'   => 'Font Family',
                'type'    => 'text',
                'default' => '"Titillium Web",Arial,sans-serif',
                'comment' => 'The font family to use for the main body text.',
                'rules'   => 'required|string',
            ],
        ]
    ]
];
```

In this example, a text field is defined for customizing the font family. The field has a label, a default value, a comment, and a validation rule. 

Once the fields are defined, you can access the values inside any of your theme templates using $this->theme->field_name. For example:

```blade
<h1 style="font-family: {{ $this->theme->font_family }}">Welcome to our website!</h1>
```

In this example, the font family defined in the theme settings interface is applied to a heading.

## Best practices

- All template files (layouts, pages and partials) should use `.blade.php`.
- Use relative paths in your CSS files, for example: `url(../img/bg.png);`
- Use lowercase filenames
- Use hyphens `-` **NOT** underscores `_` to separate words in filenames
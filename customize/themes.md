---
title: "Themes"
section: customize
sortOrder: 100
---

## Introduction

TastyIgniter themes are files that work together to create a TastyIgniter site's design and functionality. Each theme can be different, offering site owners many choices to change their website look instantly. This article is about developing themes for TastyIgniter. 

Themes are by default subdirectories in the **/themes** directory. The subdirectory of the theme contains all the [pages](../customize/pages), [partials](../customize/partials), [layouts](../customize/layouts) and [assets](../customize/media-files) files and optional theme PHP file (theme.php).

> The active theme is set in the config/system.php file with the defaultTheme parameter or in the **Design > Themes** Admin page with the Theme Selector. 

## Directory structure

Below is an example of a theme directory structure. Every TastyIgniter theme is represented by way of a separate directory and typically a theme is activated to display the website.

```yaml
themes/
  your-theme/           <=== Theme starts here
    _layouts/         	<=== Layouts directory
      default.php
    _pages/           	<=== Pages directory contains the website pages
      home.php
	_partials/        	<=== Partials directory contains reusable HTML chucks
      sidebar.php
    _meta/        		<=== Meta directory
      assets.json			<=== Registers global css, js files and HTML meta tags 
    _content/         	<=== Content directory contains reusable HTML blocks
      cta.php
    assets/          	<=== Assets directory contains images, CSS and JavaScript files.
      css/
      js/
      images/
    screenshot.png
  	theme.json        	<=== Manifest file
  	theme.php
```

TastyIgniter supports a single level subdirectory for layouts, pages, partials and content files (any structure can be used in the assets directory), which simplifies organizing of large websites. 

A theme can contain any number of other subdirectories as well. 

### Theme manifest file (optional)

A `theme.json` file looks like this:

```json
{
  "code": "tastyigniter-orange",
  "name": "TastyIgniter Orange",
  "description": "This is a front-end theme",
  "author": "Dev Team",
  "version": "0.1.0"
}
```

| Field           | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| **code**        | the theme code, required. The value is used on the TastyIgniter marketplace for setting the theme code value. |
| **name**        | specifies the theme name, required.                          |
| **description** | the theme description, required.                             |
| **author**      | specifies the author name, required.                         |
| **version**     | specifies the theme version, required.                       |
| **homepage**    | specifies the author website URL, optional.                  |
| **require**     | an array of extension names the theme depeneds on, optional. |

### Assets manifest file

Before adding your files to the theme assets manifest file, you must place them within your theme in the correct directory structure, as shown in the [theme directory structure](#directory-structure).

A `_meta/assets.json` file looks like this:

```json
{
  "doctype": "html5",
  "favicon": "favicon.ico",
  "meta": [
    //...
  ],
  "css": [
    {
      "path": "theme/assets/css/app.css",
      "name": "app-css"
    }
  ],
  "js": [
    {
      "path": "theme/assets/js/app.js",
      "name": "app-js"
    }
  ]
}
```

JavaScript code should be placed in external files whenever possible. Use `get_script_tags()` to load your scripts.

### Screenshot

Create a screenshot for your theme. The screenshot should be named `screenshot.png` and should be placed in the top level directory. The recommended image size is 1200px wide by 900px tall. The screenshot is usually smaller but the over-size image allows high-resolution viewing on HiDPI displays. 

## Template structure

A template is how a page is organized, and how the content of the page is displayed. Both layouts and pages templates may contain up to three sections: front-matter, PHP code and HTML markup. Sections are separated by the `---` sequence. For example:

```php+HTML
---
title: Menu Items
permalink: "/menus"
layout: default

'[cartBox]':
    showCartItemThumb: 1
---
<?php
function onStart()
{
    //...
}
?>
---
<div>
    <?= component('cartBox') ?>
</div>
```

### Front-matter section

The front matter must be the first thing in the file and must take the form of a valid YAML set between triple-dashed lines. You can set predefined variables between these triple-dashed lines (see below for a reference) or even create custom ones of your own. These variables will then be available to you to access using PHP tags both further down in the file and also in any template referenced. Here is a basic example:

```yaml
---
title: "Menu Items"
permalink: "/menus"
layout: "default"

'[cartBox]':
    parameter: "value"
---
```

### PHP code section

The code within the PHP section is executed each time before the template is rendered. The PHP section is optional and the content depends on the type of template it is defined within. The PHP open and close tags should always be set within the section separator `---` on a different line. For example:

```php+HTML
---
<?php
use Acme\Menu\Models\Category;

function onStart() {
    $this['categories'] = Category::all();
}
?>
---
<h3>Categories</h3>
<ul>
    <?php foreach ($categories as $category) { ?>
	    <li><?= $category->name; ?></li>
	<?php } ?>
</ul>
```

The PHP section is converted to a PHP class when the template is parsed, you can only define functions and `use` namespaces, no other PHP code is allowed. You can access `$this` object to read and set variables.

### HTML markup

The HTML section defines the markup to be rendered by the template. The HTML section content can contain both HTML and PHP tags, functions, the content depends on whether the template is a page, layout or partial.

## Including Template files

To load a partial or content file from another template file, you can use `partial('my-partial-name')` to make it easy for a theme to reuse code sections. The template paths are always absolute. If you render a partial from the same subdirectory `blog/`, the subdirectory name still needs to be specified, `partial('blog/my-partial-name')`.

## Global Variables

| VARIABLE       | DESCRIPTION                                                  |
| -------------- | ------------------------------------------------------------ |
| **theme**      | Theme object `$this->theme` for reading customization settings. |
| **page**       | Page object `$this->page`. Custom variables set via front matter in [pages](../customize/pages) will be available here. |
| **layout**     | Layout object `$this->layout`. Custom variables set via front matter in [layouts](../customize/layouts) will be available here. |
| **controller** | Access the underlying controller object `$this->controller`. |

## Customization API

The Theme Customization feature is available by default for almost all TastyIgniter themes from **Design > Themes**. The Theme Customization Admin page is automatically populated with form fields that a theme registers using the `_meta/fields.php` file.

> Please note that the Customize Screen is only available if the active theme supports Customization. Furthermore, this screen will probably be different for each theme that enables and builds it. 

Example of a `_meta/fields.php` file:

```php
return [
    // Set form fields for the admin theme customisation.
    'form' => [
        'fields'  => [
            'font_family' => [
                'label'   => 'Font Family',
                'type'    => 'text',
                'default' => '"Titillium Web",Arial,sans-serif',
                'comment' => 'The font family to use for the main body text.',
                'rules'   => 'required',
            ],
        ]
    ]
];
```

The value can then be accessed inside any of the Theme templates:

```php+HTML
<h1>Welcome to <?= $this->theme->font_family; ?>!</h1>
```

## Best Practices

- Name your main CSS and JavaScript files the same name as your theme
- All template files (layouts, pages, partials, content) should use `.php` .
- Use relative paths in your CSS files, for example: `url(../img/bg.png);`
- Use lowercase filenames
---
title: "Pages"
section: customize
sortOrder: 110
---

## Introduction

All websites are made up of pages. In TastyIgniter, pages are the most basic building block for content. Page file names or directory structure do not affect the routing, but naming your pages according to the page function is a good idea.

Pages files live in the **/_pages** subdirectory of a theme directory. 

```yaml
themes/
  your-theme/           <=== Theme directory
    _pages/         	<=== Pages subdirectory
      home.php			<=== Page template file
```

The PHP code section is optional, but both front matter and HTML markup sections are required. For example:

```yaml
---
title: My First Page
description: "My first page example"
permalink: "/page"
layout: default
---
<p>This is the content of my page</p>
```

## Page front matter

A number of predefined parameters can be set in the front-matter of a page.

| VARIABLE    | DESCRIPTION                                                  |
| ----------- | ------------------------------------------------------------ |
| `title`     | Use this variable as the page title.                         |
| `permalink` | Set this variable and use it as the page URL.                |
| `layout`    | If set, this specifies the layout file to be used. Use the name of the layout file without the file extension. |

You can also set your own front-matter variables accessible in PHP. For example, if you set a food variable, you can use it on your page:

```php+HTML
---
food: "Pizza"
---
<h1><?= $this->page->food; ?></h1>
```

## Permalink syntax

The simplest way to set a permalink is using front matter. You set the `permalink` variable in the front matter to the URL you’d like. Permalinks should start with the forward slash character and can contain parameters.

For example, you might have a page on your site located at `/_pages/common/about.php` and you want the url to be `/about`. In front matter of the page you would set:

```yaml
permalink: "/about"
```

### Permalink parameters

For any address such as `/pages/my-first-page`, a page with the **permalink pattern** defined in the following example would be displayed.

```yaml
permalink: "/pages/:title" - this will match /pages/page-title
```

To make a parameter optional add the **question mark** after its name:

```yaml
permalink: "/pages/:title?" - this will match /pages/page-title OR /pages
```

Parameters can not be optional in the **middle** of the permalink:

```yaml
permalink: "/pages/:title?/:slug" - this will match /pages/page-title/page-slug
```

A default value can be specified after the **question mark** and used as fallback values in case the real parameter value is not presented. **Default values can not contain any asterisks, pipe symbols or question marks.** 

```yaml
permalink: "/pages/:title?default" - this will match /pages/page-title OR /pages/default
```

You could also use regular expressions to validate parameters:

```yaml
permalink: "/pages/:title|^[a-z0-9\-]+$" - this will match /pages/page-title
```

A special wildcard parameter can be used by placing the asterisk after the parameter:

```yaml
permalink: "/pages/:title*/:slug" - this will match /pages/page-title/child/page/page-slug
```

## Execution life cycle

In the PHP code section of pages and layouts there are specific functions: `onInit`, `onStart` and `onEnd`. 

The function `onInit` is executed when all [components]({{site.baseurl}}/customize/components) are initialized and before AJAX requests are handled. The `onStart` function is executed at the start of the execution of the page. The `onEnd` function is executed before the page is rendered and the page components are executed. 

```php+HTML
---
<?php
function onEnd()
{
    $this['hello'] = "Hello world!";
}
?>
---
<h3><?= $this->page->hello; ?></h3>
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

The controller's `addCss` and `addJs` methods allow you to inject assets files (CSS and JavaScript) to pages. This can be done in the onStart function defined in a page or layout template PHP section.

Asset path that **starts with a slash (/)** is relative to the root of your website, but relative to the theme, if the assets path **does not start with a slash (/)**.

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

In order to output the injected assets on pages or [layouts]({{site.baseurl}}/customize/layouts) use the `<?= get_style_tags(); ?>` and `<?= get_script_tags(); ?>` tags. Example:

```php+HTML
<head>
    ...
    <?= get_style_tags(); ?>
</head>
<body>
    ...
    <?= get_script_tags(); ?>
</body>
```

> The page output in the above example will include all assets files registered within the `_meta/assets.json` manifest file.
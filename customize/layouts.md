---
title: "Layouts"
section: customize
sortOrder: 120
---

## Introduction

Layouts are templates that wrap around your content. They allow you to have your template source code in one place so that you don't have to repeat things like your header and footer on every page. 

Layouts files live in the **/_layouts** subdirectory of a theme directory. 

```yaml
themes/
  your-theme/           <=== Theme directory
    _layouts/         	<=== Layouts subdirectory
      default.php		<=== Layout template file
```

The convention is to have a basic layout called `default.php` and be used by other pages as required. Within the layout file, you should use the `<?= page(); ?>`  tag to display the content of the page.

```php+HTML
<!DOCTYPE html>
<html>
    <head>
        <!-- -->
    </head>
    <body>
        <?= page(); ?>
    </body>
</html>
```

Next you need to specify what layout to use in your page's front matter.

```yaml
---
permalink: "/page"
layout: default
---
<p>This is the content of my page</p>
```

## Layout front matter

The front matter section is optional for layouts. The optional front matter parameters are `name` and `description` and are used in the Admin user interface. For example:

```yaml
---
name: My First Layout
description: My first layout example
---
<!DOCTYPE html>
<html>
    <head>
        <!-- -->
    </head>
    <body>
        <?= page(); ?>
    </body>
</html>>
```

You can set the front matter in layouts, the only difference is you use the layout object instead of the page. For example:

```php+HTML
---
food: "Pizza"
---
<p><?= $this->layout->food; ?></p>

<?= page(); ?>
```

## Execution life cycle

Specific functions can be defined in the layouts PHP code section for handling the page execution life cycle: `onInit`, `onStart` and `onEnd`. 

The `onInit` function is executed when all [components]({{site.baseurl}}/customize/components) are initialized and before AJAX requests are handled. The `onStart` function is executed at the start of the execution of the page. The `onEnd` function is executed after the page is rendered.

```php+HTML
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
<h3><?= $this->layout->hello; ?></h3>
```

## 
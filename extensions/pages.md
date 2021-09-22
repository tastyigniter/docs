---
title: "Pages Extension"
section: "extensions"
sortOrder: 70
---

## Introduction

This TastyIgniter extension allows end users to manage static pages and menus with a simple WYSIWYG user interface.

This extension includes two components: Static Page and Static Menu.

## Installation

To install this extension, click on the **Add to Site** button on the TastyIgniter marketplace item page or search for **Igniter.Local** in **Admin System > Updates > Browse Extensions**

## Admin Panel

In the admin user interface you can create, edit or delete pages and menus.

## Usage

The first thing we have to do is create the layout that will host all pages of the website.

```php+HTML
---
description: Static layout for static pages

'[staticPage]':

'[staticMenu]':
    code: main-menu
---
<html>
    <head>
        <title>{{ $this->page->title }}</title>
    </head>
    <body>
        @component('staticMenu')
        {!! page() !!}
    </body>
</html>
```

Includes the staticPage and staticMenu component to the layout. The staticMenu component has the `code` property that should correspond to a code of the static menu to display.

The static menu component injects the `menuItems` page variable. The `menuItems` variable is an array of objects. Each object has the following properties:

- `title` - specifies the menu item title.
- `url` - specifies the absolute menu item URL.
- `isActive` - indicates whether the item corresponds to a page currently being viewed.
- `isChildActive` - indicates whether the item contains an active subitem.
- `extraAttributes` - specifies the menu item extra HTML attributes
- `items` - an array of the menu item subitems, if any. If there are no subitems, the array is empty

```php+HTML
@foreach ($menuItems as $item)
   <li><a href="{{ $item->url }}">{{ $item->title }}</a></li>
@endforeach
```



**Setting the active menu item explicitly**

You may want to tag a particular menu item explicitly as active in some cases. You can do that in the `onInit()` function of the page, by assigning a value to the `activeMenuItem` page variable matching the menu item code that you want to activate. In the Edit Menu Item popup, menu item codes are managed.

```php
function onInit()
{
    $this['activeMenuItem'] = 'about-us';
}	
```
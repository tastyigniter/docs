---
title: "Components"
section: customize
sortOrder: 140
---

## Introduction

TastyIgniter Components implements content and features that extend your website. They can be added, removed, and rearranged from **Design > Themes > Editor** in the Administration Panel.

Other than displaying HTML markup on a page, components can implement the handling of [AJAX requests](../advanced/ajax-request), the handling of postbacks and the handling of the page execution life cycle, which enables pages to be injected.

This article describes using components within themes and does not explain [building components](../extend/building-components) as part of extensions.

## Attaching components

Use the admin interface to attach components to pages and layouts. You can attach a component to a page or layout manually using a file editor by following the example below:

```yaml
---
title: My first page
permalink: "/page"

'[cartBox]':
  showCartItemThumb: 1
---
```

The **cartBox** component in the above example initializes the property **showCartItemThumb** with value **1**.

When you attach a component, a page variable that matches the component name is automatically created (`$cartBox` in the previous example). Render components HTML markup on a page or layout as follows:

```php+HTML
@component('cartBox')
```

### Using multiple instances of the same component

If two components with the same name are assigned to a page and layout together, the page component overrides any properties of the layout component.

You can attach components of the same name registered within two extensions by using its fully qualified class name and assigning it an *alias*:

```php+HTML
'[Igniter\Cart\Components\CartBox cartBox]':
    showCartItemThumb: 1

'[Igniter\Local\Components\CartBox localCartBox]':
    showCartItemThumb: 1
---
@component('cartBox')

@component('localCartBox')
```

Define multiple instances of a same component:

```php+HTML
'[cartBox cartBoxA]':
    showCartItemThumb: 1

'[cartBox cartBoxB]':
    showCartItemThumb: 1
---
@component('cartBoxA')

@component('cartBoxB')
```

## Component variables

The  `@component('cartBox')` tag accepts an array of variables as its second parameter. The specifed variables will be available at the time of rendering and will explicitly override the component property values:

```php+HTML
@component('cartBox', ['showCartItemThumb' => 0])
```

The **showCartItemThumb** property value of the component will be set to **0** when the component is being rendered.

## Overriding component partials

All component partials can be overridden by creating theme partials. A component defined with alias **cartBox** can have its **control.blade.php** partial overridden by creating a theme file called **_partials/cartBox/control.blade.php**


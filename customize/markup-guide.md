---
title: "Markup Guide"
section: customize
sortOrder: 170
---

## Introduction

TastyIgniter adds a number of directives and variables to the <a href="https://laravel.com/docs/blade" targer="_blank">Blade template engine</a>. 

## Variables

### $this->page

### $this->layout

### $this->theme

### $this->param


## Blade Directives

### @page

The `@page` tag renders the contents of a page into a layout template.

```php+HTML
@page
```

### @partial

```php+HTML
@partial('footer')
```

### @component

```php+HTML
@component('cartBox')
```



```php+HTML
@component('cartBox', ['checkStockCheckout' => TRUE])
```



```php+HTML
@partial('cart::cartBox')
```

### @hasComponent

```php+HTML
@hasComponent('cartBox')
```

### @content

```php+HTML
@content('welcome.htm')
```



### @stack

```php+HTML
@push('sidebar')
	Add this content to the sidebar
@endpush
```



```php+HTML
@stack('sidebar')
```

### @prepend

```php+HTML
@prepend('sidebar')	
	Add this content as the first
@endprepend
```

### @styles

```php+HTML
@styles
```

### 

```php+HTML
@push('styles')
		<link rel="stylesheet" href="/css/app.css">
@endpush
```

### @scripts

```php+HTML
@stack('footer')
```



```php+HTML
@push('styles')
		<link rel="stylesheet" href="/css/app.css">
@endpush
```

### @inject

```php+HTML
@inject('metrics', 'App\Services\MetricsService')

<div>
    Monthly Revenue: {% raw %}{{ $metrics->monthlyRevenue() }}{% endraw %}.
</div>
```



## Unsupported Directives

### 

| Directive          | Equivalent |
| ------------------ | ---------- |
| `@extends`           | Use [Theme Layouts]() |
| `@include`           | Use `@partial`           |
| `@includeIf`         | Use `@partial`           |
| `@includeWhen`       | Use `@partial`           |
| `@includeUnless`     | Use `@partial`           |
| `@includeFirst`      | Use `@partial`           |
| `@include`           | Use `@partial`           |
| `@endcomponent`      | Use `@component` |
| `@componentfirst`    | Use `@component` |
| `@endcomponentfirst` | Use `@`component` |



```

```
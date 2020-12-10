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


## Directives

Directives are a unique feature to Laravel Blade and are wrapped with `{{ }}` characters or prefixed with `@` character.

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

### @content

```php+HTML
@content('welcome.htm')
```

### @partialIf

```php+HTML
@partialIf('cartBox')
```

### @hasComponent

```php+HTML
@hasSection('navigation')
    <div class="pull-right">
        @yield('navigation')
    </div>

    <div class="clearfix"></div>
@endif
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

```php+HTML
@push('scripts')
    This will be second...
@endpush

// Later...

@prepend('scripts')
    This will be first...
@endprepend
```

### @styles

```php+HTML
@styles
```

```
function onStart()
{
    $this->addCss('assets/css/style.css');
}
```

```php+HTML
@push('styles')
		<link rel="stylesheet" href="assets/css/app.css">
@endpush
```

### @scripts

```php+HTML
@scripts
```

```
function onStart()
{
    $this->addJs('assets/js/script.js');
}
```

```php+HTML
@push('scripts')
		<script src="assets/js/app.js"></script>
@endpush
```

### @verbatim

```php+HTML
Hello, @{{ name }}.
```

```php+HTML
@verbatim
    <div class="container">
        Hello, {{ name }}.
    </div>
@endverbatim
```

### @if

```php+HTML
@if (count($categories) === 1)
    I have one category!
@elseif (count($categories) > 1)
    I have multiple categories!
@else
    I don't have any categories!
@endif
```

### @auth

```php+HTML
@auth
    // The user is authenticated...
@endauth

@guest
    // The user is not authenticated...
@endguest
```

### @unless

```php+HTML
@unless (Cart::content())
    Cart is empty.
@endunless
```

### @for

```php+HTML
@for ($i = 0; $i < 10; $i++)
    The current value is {{ $i }}
@endfor

@foreach ($categories as $category)
    <p>This is category {{ $category->name }}</p>
@endforeach

@forelse ($categories as $category)
    <li>{{ $category->name }}</li>
@empty
    <p>No categories</p>
@endforelse

@while (true)
    <p>I'm looping forever.</p>
@endwhile
```

### @inject

```php+HTML
@inject('metrics', 'App\Services\MetricsService')

<div>
    Monthly Revenue: {{ $metrics->monthlyRevenue() }}.
</div>
```



## Unsupported Directives

### 

| Directive          | Equivalent |
| ------------------ | ---------- |
| `@extends`           | Use [Theme Layouts]() |
| `@include`           | Use `@partial`           |
| `@includeIf`         | Use `@partialIf`         |
| `@includeWhen`       | Use `@partialWhen`       |
| `@includeUnless`     | Use `@partialUnless`     |
| `@includeFirst`      | Use `@partialFirst`      |
| `@endcomponent`      | Use `@component` |
| `@componentfirst`    | Use `@component` |
| `@endcomponentfirst` | Use `@`component` |



```

```
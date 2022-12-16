---
title: "Markup Guide"
section: customize
sortOrder: 170
---

## Introduction

TastyIgniter adds a number of directives and variables to the <a href="https://laravel.com/docs/blade" target="_blank">
Blade template engine</a>.

## Variables

|                 |
| --------------- |
| `$this->page`   |
| `$this->layout` |
| `$this->theme`  |
| `$this->param`  |

## Directives

Directives are a unique feature to Laravel Blade and are wrapped with `{{ }}` characters or prefixed with `@` character.

### @page

The `@page` tag renders the contents of a page into a layout template.

```blade
@page
```

### @partial

```blade
@partial('footer')
```

### @component

```blade
@component('cartBox')
```

```blade
@component('cartBox', ['checkStockCheckout' => TRUE])
```

```blade
@partial('cart::cartBox')
```

### @content

```blade
@content('welcome.htm')
```

### @partialIf

```blade
@partialIf('cartBox')
```

### @hasComponent

```blade
@hasSection('navigation')
    <div class="pull-right">
        @yield('navigation')
    </div>

    <div class="clearfix"></div>
@endif
```

### @stack

```blade
@push('sidebar')
	Add this content to the sidebar
@endpush
```

```blade
@stack('sidebar')
```

```blade
@push('scripts')
    This will be second...
@endpush

// Later...

@prepend('scripts')
    This will be first...
@endprepend
```

### @styles

```blade
@styles
```

```php
function onStart()
{
    $this->addCss('assets/css/style.css');
}
```

```blade
@push('styles')
		<link rel="stylesheet" href="assets/css/app.css">
@endpush
```

### @scripts

```blade
@scripts
```

```php
function onStart()
{
    $this->addJs('assets/js/script.js');
}
```

```blade
@push('scripts')
		<script src="assets/js/app.js"></script>
@endpush
```

### @verbatim

```blade
Hello, @{{ name }}.
```

```blade
@verbatim
    <div class="container">
        Hello, {{ name }}.
    </div>
@endverbatim
```

### @if

```blade
@if (count($categories) === 1)
    I have one category!
@elseif (count($categories) > 1)
    I have multiple categories!
@else
    I don't have any categories!
@endif
```

### @auth

```blade
@auth
    // The user is authenticated...
@endauth

@guest
    // The user is not authenticated...
@endguest
```

### @unless

```blade
@unless (Cart::content())
    Cart is empty.
@endunless
```

### @for

```blade
@for ($i = 0; $i < 10; $i++)
    The current value is {{ '{{ $i }}' }}
@endfor

@foreach ($categories as $category)
    <p>This is category {{ '{{ $category->name }}' }}</p>
@endforeach

@forelse ($categories as $category)
    <li>{{ '{{ $category->name }}' }}</li>
@empty
    <p>No categories</p>
@endforelse

@while (true)
    <p>I'm looping forever.</p>
@endwhile
```

### @inject

```blade
@inject('metrics', 'App\Services\MetricsService')

<div>
    Monthly Revenue: {{ '{{ $metrics->monthlyRevenue() }}' }}.
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
| `@endcomponentfirst` | Use `@component` |



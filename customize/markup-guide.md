---
title: "Markup Guide"
section: customize
sortOrder: 170
---

## Introduction

TastyIgniter adds a number of directives and variables to the [Blade template engine](https://laravel.com/docs/blade). 

## Variables

### $this->page

### $this->layout

### $this->theme

### $this->param


## Blade Directives

### @content

```php+HTML
@content('welcome.htm')
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

### @page

The `@page` tag renders the contents of a page into a layout template.

```php+HTML
@page
```

### @hasComponent

```php+HTML
@hasComponent('cartBox')
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

### @placeholder

```php+HTML
@placeholder('name')
```

### 

```php+HTML
@push('name')
		This is a placeholder content
@endpush
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
@partial('footer')
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

### @include

```php+HTML
@include('view.name', ['some' => 'data'])
```

### @includeIf

```php+HTML
@includeIf('view.name', ['some' => 'data'])
```

### @includeWhen

```php+HTML
@includeWhen($boolean, 'view.name', ['some' => 'data'])
```

### @includeFirst

```php+HTML
@includeFirst(['custom.admin', 'admin'], ['some' => 'data'])
```


---
title: "Partials"
section: customize
sortOrder: 130
---

## Introduction

Partials include reusable Blade markup blocks that can be used anywhere on the website. Partials are extremely useful for page sections that appear on multiple pages or layouts.

While you can use Blade's `@include` directive, [components](../customize/components) provide similar functionality and have several advantages over the `@include` directive, including data and attribute binding.

Partial files live in the **resources/views/includes** subdirectory of a theme directory. For example:

## Creating a partial

To create a partial, you need to create a new `.blade.php` file in the `resources/views/includes` directory of your theme. The name of the file will be the name of the partial. For example, to create a partial named `header`, you would create a file named `header.blade.php`.

```yaml
acme/                           <=== Theme vendor directory
  purple/                       <=== Theme directory
    resources/
      views/
        includes/               <=== Partials subdirectory
          header.blade.php      <=== Partial template file
```

Inside this file, you can write any Blade markup that you want to reuse across your theme.

## Rendering partials

The Blade directive `@include('partial-name')` renders a partial. The directive has a single parameter that is required - the name of the partial file without the `.blade.php` extension. You can specify the name of the subdirectory if you refer a partial from a subdirectory `@include('directory.partial-name')`. You may use the `@include` directive within a page, layout or other partials. An example of a page rendering a partial:

```blade
<div class="sidebar">
    @include('sidebar')
</div>
```

### Passing data to partials

You may pass variables to partials by defining them in the `@include` directive after the partial name:

```blade
<div class="sidebar">
    @include('sidebar', ['pages' => $pages])
</div>
```

You can access variables within the partial like any other markup variable: 

```blade
<ul>
	@foreach($pages as $page)
		<li>{{ $page->name }}</li>
	@endforeach
</ul>
```

For more information, the [Including Subviews section of the Laravel Blade documentation](https://laravel.com/docs/blade#including-subviews) provides a good starting point.

## Overriding a component partial

To override a component partial in a theme, you need to create a new `.blade.php` file with the same name as the partial you want to override, in the `resources/views/_components` directory of your theme. The content of this file will be used instead of the original partial.

For example, to override the `default` partial for component `acme.hello-world::hello-block`, you would create a file named `hello-block.blade.php` in the `resources/views/_components/hello-block.blade.php` directory of your theme.

## Using partials within components

Partials can be used with components to create reusable pieces of functionality. To use a partial within a component, you can use the `@include` directive, followed by the name of the partial.

For example, to include the `header` partial within a component, you would use the following code:

```blade
@include('header')
```

This will render the header partial wherever the component is used.

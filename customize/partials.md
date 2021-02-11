---
title: "Partials"
section: customize
sortOrder: 130
callout: This section is incomplete. Please help to improve it.
---

## Introduction

Partials include reusable Blade markup blocks that can be used anywhere on the website. Partials are extremely useful for page sections that appear on multiple pages or layouts.

Partial files live in the **/_partials** subdirectory of a theme directory.

```yaml
themes/
	your-theme/           <=== Theme directory
		_partials/         	  <=== Partials subdirectory
			sidebar.blade.php     <=== Partial template file
```

## Rendering partials

The Blade directive `@partial('partial-name')` renders a partial. The directive has a single parameter that is required - the name of the partial file without the `.blade.php` extension. You can specify the name of the subdirectory if you refer a partial from a subdirectory `@partial('directory/partial-name')`. You may use the `@partial` directive within a page, layout or other partials. An example of a page rendering a partial:

```php+HTML
<div class="sidebar">
    @partial('sidebar')
</div>
```

## Passing parameters to partials

You may pass variables to partials by defining them in the `@partial` directive after the partial name:

```php+HTML
<div class="sidebar">
    @partial('sidebar', ['pages' => $pages])
</div>
```

You can access variables within the partial like any other markup variable: 

```php+HTML
<ul>
	@foreach($pages as $page)
		<li>{{ $page->name }}</li>
	@endforeach
</ul>
```


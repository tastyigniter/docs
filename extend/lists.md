---
title: "Lists"
section: extend
sortOrder: 240
---

## Introduction

The `Igniter\Admin\Http\Actions\ListController` provide a way to display a list of records from a model and handle user interaction such as searching, sorting, filtering, and pagination.

## Configuring the list controller

To use the `ListController` in your extension, you need to add it to the controller's `$implement` property. Also, you should define a `$listConfig` property in your controller and its value should be an array of configuration options. Here is an example of a list controller configuration:

```php
namespace Author\Extension\Http\Controllers;

class MyController extends \Igniter\Admin\Classes\AdminController
{
    public array $listConfig = [
        'model' => 'Author\Extension\Models\Record',
        'title' => 'Records',
        'emptyMessage' => 'No records found',
        'defaultSort' => ['id', 'DESC'],
        'configFile' => 'record',
    ];
}
```

The following fields are required in the list configuration:

- `model` - The model class name to use for the list.
- `configFile` - Reference to the [list definition file](../extend/lists#list-definition-file).

The configuration options listed below are optional.

- `title` - The title of the list.
- `emptyMessage` - The message to display when no records are found.
- `showSorting` - Whether to show sorting controls. Default is `true`.
- `defaultSort` - The default sort order for the list. The value should be an array with the column name and sort direction.
- `pageLimit` - The number of records to display per page. Default is `20`.
- `showPageNumbers` - Whether to show page numbers. Default is `true`.
- `showCheckboxes` - Whether to show checkboxes for each record.
- `showSetup` - Whether to show the setup button. Default is `true`.

### List definition file

The list definition file is typically located in the extension's `resources/models` directory. The list definition file should return a `list` array of [toolbar buttons](../extend/lists#toolbar-button-options), [filter scopes](../extend/lists#scope-options) and [lists columns](../extend/lists#column-options) definitions. For example:

```php
return [
    'list' => [
        'toolbar' => [
            'buttons' => [
                // ...
            ],
        ],
        'filter' => [
            'search' => [
                'prompt' => 'Search records',
                'mode' => 'all',
            ],
            'scopes' => [
                // ...
            ],
        ],
        'columns' => [
            // ...
        ],
    ],
    // ...
];
```

## Adding a Toolbar

You can add a toolbar to the list controller by defining a `toolbar` key in the list definition file. The toolbar configuration should be an array of toolbar buttons. Here is an example of a list controller with a toolbar defined in the list definition file `resources/models/record.php`:

```php
return [
    'list' => [
        'toolbar' => [
            'buttons' => [
                'new' => [
                    'label' => 'New Record',
                    'class' => 'btn btn-primary',
                    'href' => 'author/extension/mycontroller/create',
                ],
            ],
        ],
    ],
];
```

### Toolbar button options

For each toolbar button, you can specify these options:

- `type` - The type of button to display. Available types are: `link`, `button`, `dropdown`. Default is `link`.
- `label` - The label to display for the button.
- `context` - The context to apply to the button. Default is `primary`.
- `disabled` - Whether the button is disabled. Default is `false`.
- `partial` - The partial view to render when the button is clicked.
- `permissions` - The [permissions](../extend/permissions) required to access the button.
- `class` - The CSS class to apply to the button.
- `href` - The URL to link to when the button is clicked.

## Filtering the list

You can filter the list by defining a `filter` key in the list definition file. The filter configuration should be an array of the search configuration and filter scopes. Here is an example of a list controller with a filter defined in the list definition file `resources/models/record.php`:

```php
return [
    'list' => [
        'filter' => [
            'search' => [
                'prompt' => 'Search records',
                'mode' => 'all',
            ],
            'scopes' => [
                'status' => [
                    'label' => 'Status',
                    'type' => 'switch',
                    'conditions' => 'status = :filtered',
                ],
            ],
        ],
    ],
];
```

### Search options

For the search filter, you can specify these options:

- `prompt` - The placeholder text to display in the search input.
- `mode` - The search mode to use. Available modes are: `all`, `any`, `exact`. Default is `all`.
- `scope` - The query scope method defined in the list model to apply to the search query.

### Scope options

For each scope you can specify these options (where applicable):

- `label` - The label to display for the filter scope.
- `type` - The type of filter scope to display.
- `conditions` - The raw where query statement to apply to the list query. `:filtered` parameter represents the filtered value(s).
- `default` - The default value for the filter scope.
- `options` - An array of options to display in the filter scope dropdown.
- `scope` - The query scope method defined in the list model to apply to the list query.
- `nameFrom` - The model attribute to use as the label of the filter scope.
- `permissions` - The [permissions](../extend/permissions) required to access the filter scope.

### Available filter scope types

These types can be used to determine how the filter scope should be displayed.

- [Switch](../extend/lists#switch)
- [Select](../extend/lists#select)
- [Select list](../extend/lists#select-list)
- [Date](../extend/lists#date)
- [Date range](../extend/lists#date-range)
- [Checkbox](../extend/lists#checkbox)

#### Switch

`switch` used as a switch to toggle between two predefined conditions.

```php
'status' => [
    'label' => 'Status',
    'type' => 'switch',
    'default' => 1,
    'conditions' => ['status <> true', 'status = :filtered'],
],
```

#### Select

`select` used as a dropdown to apply a predefined condition to the list.

```php
'status' => [
    'label' => 'Status',
    'type' => 'select',
    'conditions' => 'status = :filtered',
    'options' => [
        'active' => 'Active',
        'inactive' => 'Inactive',
    ],
],
```

#### Select list

`selectlist` used as a dropdown to apply one or more predefined conditions to the list.

```php
'status' => [
    'label' => 'Status',
    'type' => 'selectlist',
    'conditions' => 'status = :filtered',
    'options' => [
        'active' => 'Active',
        'inactive' => 'Inactive',
    ],
],
```

#### Date

`date` displays a date picker for a single date to be selected and applied to the list.

```php
'date' => [
    'label' => 'Date',
    'type' => 'date',
    'conditions' => 'date = :filtered',
],
```

#### Date range

`daterange` - Date range picker

#### Checkbox

`checkbox` - Checkbox input

## Defining list columns

You can define the columns to display in the list by defining a `columns` key in the list definition file. The columns configuration should be an array of column definitions. Here is an example of a list controller with columns defined in the list definition file `resources/models/record.php`:

```php
return [
    'list' => [
        'columns' => [
            'name' => [
                'label' => 'Name',
                'searchable' => true,
            ],
            'email' => [
                'label' => 'Email',
                'searchable' => true,
            ],
        ],
    ],
];
```

### Column options

For each column can specify these options (where applicable):

- `label` - The label to display for the column.
- `type` - The type of column to display (see [Column types](../extend/lists#available-column-types) below). Default is `text`.
- `default` - The default value for the column.
- `searchable` - Whether the column is searchable. Default is `false`.
- `sortable` - Whether the column is sortable. Default is `true`.
- `invisible` - Whether the column is visible. Default is `false`.
- `select` - Defines a custom SQL select statement to use for the value.
- `relation` - The model relationship name to use to fetch the column value.
- `valueFrom` - The model attribute to use as the column value.
- `cssClass` - The CSS class to apply to the column container.
- `width` - The width of the column. Default is `auto`.
- `permissions` - The [permissions](../extend/permissions) required to access the column.

### Available column types

There are various column types that can be used to control how the list column is displayed.

- [Text](../extend/lists#text)
- [Switch](../extend/lists#switch-1)
- [Money](../extend/lists#money)
- [Currency](../extend/lists#currency)
- [Date](../extend/lists#date-1)
- [Time](../extend/lists#time)
- [Datetime](../extend/lists#datetime)
- [Time since](../extend/lists#time-since)
- [Time tense](../extend/lists#time-tense)
- [Date since](../extend/lists#date-since)
- [Partial](../extend/lists#partial)

#### Text

`text` displays a text column. The `type` key is optional and defaults to `text`.

```php
'name' => [
    'label' => 'Name',
    'searchable' => true,
],
```

#### Switch

`switch` displays on or off state for boolean columns.

```php
'is_active' => [
    'label' => 'Active',
    'type' => 'switch',
],
```

#### Money

`money` displays a column with a numeric value formatted to two decimal places.

```php
'price' => [
    'label' => 'Price',
    'type' => 'money',
],
```

#### Currency

`currency` displays a column with a numeric value formatted with a currency symbol.

```php
'price' => [
    'label' => 'Price',
    'type' => 'currency',
],
```

#### Date

`date` displays the column value as date format `MMM DD, YYYY` - **Dec 01, 2024**. You can customise this format setting the value of `format` key in the column configuration.

```php
'date' => [
    'label' => 'Date',
    'type' => 'date',
    'format' => 'MMM DD, YYYY', // Optional
],
```

#### Time

`time` displays the column value as time format `hh:mm a` - **09:05 pm**. You can customise this format setting the value of `format` key in the column configuration.

```php
'time' => [
    'label' => 'Time',
    'type' => 'time',
    'format' => 'hh:mm a', // Optional
],
```

#### Datetime

`datetime` displays the column value as datetime format `DD MMMM YYYY HH:mm` - **01 December 2024 16:05**. You can customise this format setting the value of `format` key in the column configuration.

```php
'datetime' => [
    'label' => 'Datetime',
    'type' => 'datetime',
    'format' => 'DD MMMM YYYY HH:mm', // Optional
],
```

#### Time since

`timesince` displays the human-readable time difference from the column value to the current time. Eg: **10 days ago**, **20 minutes ago**, **Just now**

```php
'timesince' => [
    'label' => 'Time Since',
    'type' => 'timesince',
],
```

#### Time tense

`timetense` displays 24-hour time and the day using the grammatical tense of the current date. Eg: **Today at 14:59**, **Yesterday at 4:00** or **18 Sep 2023 at 16:23**.

```php
'timetense' => [
    'label' => 'Time Tense',
    'type' => 'timetense',
],
```

#### Date since

`datesince` displays the column value using the grammatical tense of the current date. Eg: **Today**, **Yesterday**, **18 Sep 2023**

```php
'datesince' => [
    'label' => 'Date Since',
    'type' => 'datesince',
],
```

#### Partial

`partial` displays a partial view in the column. The `path` key is required and should be the path to the partial view file.

```php
'partial' => [
    'label' => 'Partial',
    'type' => 'partial',
    'path' => '/path/to/partial',
],
```

## Displaying the list

Typically, lists are rendered in the `index` blade view file. Because lists include the toolbar and filters, the view file will look like this:

```blade
{!! $this->renderListToolbar() !!}

{!! $this->renderListFilter() !!}

{!! $this->renderList(null, true) !!}
```

## Multiple list definitions

You can define multiple list configurations on the `$listConfig` property by using the `lists` key. Each list configuration should be an array of configuration options. Here is an example of `$listConfig` property with multiple list configurations:

```php
public array $listConfig = [
    'list1' => [
        'model' => 'Author\Extension\Models\Record',
        'title' => 'Records',
        'emptyMessage' => 'No records found',
        'defaultSort' => ['id', 'DESC'],
        'configFile' => 'record',
    ],
    'list2' => [
        'model' => 'Author\Extension\Models\Record',
        'title' => 'Records',
        'emptyMessage' => 'No records found',
        'defaultSort' => ['id', 'DESC'],
        'configFile' => 'record',
    ],
];
```

Each defined list can be rendered by passing the list name as an argument to the `renderList` method. For example:

```php
{!! $this->renderList('list1', true) !!}
```

## Extending list controller

You may need to extend the list controller to add custom functionality. There are several ways to extend the list controller:

### Extending the list configuration

You can extend the list configuration by overriding the `listExtendConfig` method in the controller. Here is an example of extending the list configuration:

```php
public function listExtendConfig($config, $listName)
{
    if ($listName === 'list1') {
        $config['toolbar']['buttons']['new'] = [
            'label' => 'New Record',
            'class' => 'btn btn-primary',
            'href' => 'author/extension/mycontroller/create',
        ];
    }
    
    return $config;
}
```

### Overriding controller action

You can override the controller action by defining a method with the same name as the action method in the controller. Here is an example of overriding the `index` action:

```php
public function index()
{
    // Call the ListController action index() method
    $this->asExtension('ListController')->index();
}
```

### Overriding views

You can override the list views by creating a new view file in the extension's `resources/views` directory. The view file should have the same name as the list view file you want to override. For example, to override the `index.blade.php` view file rendered from `MyController`, create a new view file in the `resources/views/mycontroller` directory with the same name. Here is an example adding a sidebar to the list:

```blade
<div class="row">
    <div class="col-md-3">
        <div class="sidebar">
            <!-- Sidebar content -->
        </div>
    </div>
    <div class="col-md-9">
        {!! $this->renderListToolbar() !!}
        
        {!! $this->renderListFilter() !!}
        
        {!! $this->renderList(null, true) !!}
    </div>
</div>
```

### Extending column definitions

You can extend the list columns by overriding the `listExtendColumns` method in the controller:

```php
public function listExtendColumns(Lists $widget, $model)
{
    if (!$model instanceof MyModel) {
        return;
    }

    $list->addColumns([
        'my_column' => [
            'label' => 'My Column'
        ]
    ]);
}
```

Or, you can use the `admin.list.extendColumns` event to extend the list columns from another extension:

```php
Event::listen('admin.list.extendColumns', function (Lists $widget, $model) {
    if (!$model instanceof MyModel) {
        return;
    }

    $list->addColumns([
        'my_column' => [
            'label' => 'My Column'
        ]
    ]);
});
```

### Extending filter scopes

You can extend the list filter scopes by overriding the `listFilterExtendScopes` method in the controller:

```php
public function listFilterExtendScopes(Filter $widget, $scopes)
{
    if (!$widget->getController() instanceof MyController) {
        return;
    }

    // Add a new filter scope
    $widget->addScopes([
        'my_scope' => [
            'label' => 'My Scope',
            'conditions' => 'my_column = :filtered',
        ]
    ]);
}
```

Or, you can use the `admin.filter.extendScopes` event to extend the filter scopes from another extension:

```php
Event::listen('admin.filter.extendScopes', function (Filter $widget, $scopes) {
    if (!$widget->getController() instanceof MyController) {
        return;
    }

    // Add a new filter scope
    $widget->addScopes([
        'my_scope' => [
            'label' => 'My Scope',
            'conditions' => 'my_column = :filtered',
        ]
    ]);
});
```

Additionally, you can override the `listFilterExtendScopesBefore` method in your controller or use the `admin.filter.extendScopesBefore` event to modify the filter scope definitions before any of the filter scopes object is created:

```php
public function listFilterExtendScopesBefore(Filter $widget)
{
    if (!$widget->getController() instanceof MyController) {
        return;
    }

    unset($widget->scopes['status']);
}

// Or use the event

Event::listen('admin.filter.extendScopesBefore', function (Filter $widget) {
    if (!$widget->getController() instanceof MyController) {
        return;
    }
    
    unset($widget->scopes['status']);
});
```

### Extending the list model query

You can extend the list query by overriding the `listExtendQuery` method in the controller:

```php
public function listExtendQuery($query, $alias)
{
    return $query->where('status', 'active');
}
```

Or, you can use the `admin.list.extendQuery` event to extend the list query from another extension:

```php
Event::listen('admin.list.extendQuery', function ($widget, $query) {
    if (!$widget->getController() instanceof MyController) {
        return;
    }

    return $query->where('status', 'active');
});
```

Additionally, you can override the `listExtendQueryBefore` method in your controller or use the `admin.list.extendQueryBefore` event to modify the query just after the query builder is created, and before any conditions are applied:

```php
public function listExtendQueryBefore($query, $alias)
{
    return $query->where('status', 'active');
}

// Or use the event

Event::listen('admin.list.extendQueryBefore', function ($widget, $query) {
    if (!$widget->getController() instanceof MyController) {
        return;
    }

    return $query->where('status', 'active');
});
```

### Extending the filter model query

You can extend the filter query by overriding the `listFilterExtendQuery` method in the controller:

```php
public function listFilterExtendQuery($query, $scope)
{
    if ($scope->scopeName == 'status') {
        $query->where('status', 'active');
    }
}
```

Or, you can use the `admin.filter.extendQuery` event to extend the filter query from another extension:

```php
Event::listen('admin.filter.extendQuery', function ($widget, $query, $scope) {
    if (!$widget->getController() instanceof MyController) {
        return;
    }

    if ($scope->scopeName == 'status') {
        $query->where('status', 'active');
    }
});
```

### Extending the records collection

You can extend the records collection by overriding the `listExtendRecords` method in the controller:

```php
public function listExtendRecords($records)
{
    return $records->where('status', 'active');
}
```

Or, you can use the `admin.list.extendRecords` event to extend the records collection from another extension:

```php
Event::listen('admin.list.extendRecords', function ($widget, $records) {
    if (!$widget->getController() instanceof MyController) {
        return;
    }

    return $records->where('status', 'active');
});
```

---
title: "Forms"
section: extend
sortOrder: 230
---

## Introduction

The `Igniter\Admin\Http\Actions\FormController` class provides a convenient way to create, update, and preview forms in the TastyIgniter admin interface. The `FormController` defines the actions and views needed to manage form submissions, including the ability to define form fields, validation rules, and form widgets.

## Configuring the form controller

To use the `FormController` in your extension, you need to add it to the controller's `$implement` property. Also, you should define a `$formConfig` property in your controller and its value should be an array of configuration options. Here is an example of a form controller configuration:

```php
namespace Author\Extension\Http\Controllers;

class MyController extends \Igniter\Admin\Classes\AdminController
{
    public array $formConfig = [
        'name' => 'My Form',
        'model' => 'Author\Extension\Models\Record',
        'request' => 'Author\Extension\Http\Requests\RecordRequest',
        'create' => [
            'title' => 'Create Record',
        ],
        'edit' => [
            'title' => 'Edit Record',
        ],
        'preview' => [
            'title' => 'Preview Record',
        ],
        'configFile' => 'record',
    ];
}
```

The following fields are required in the form configuration:

- `model`: The model class used to load and store form data.
- `configFile`: Reference to the [form definitions file](../extend/forms#form-definition-file).

The following fields are optional in the form configuration:

- `name`: The name of the object being managed by this form.
- `request`: The form request class used to validate form fields.
- `create`: Configuration options for the create form view.
- `edit`: Configuration options for the update form view.
- `preview`: Configuration options for the preview form view.

### Form definition file

The form definition file is typically stored in the `resources/models` directory of an extension. The form definition file should return a `form` array of [toolbar buttons](../extend/lists#toolbar-button-options) and [form fields](../extend/lists#scope-options). For example:

```php
return [
    'form' => [
         'toolbar' => [
            'buttons' => [
                // ...
            ],
        ],
        'fields' => [
            // ...
        ],
    ],
];
```

### Create page

To support the Create page add the following configuration to the form configuration:

```php
public array $formConfig = [
    // ...
    'create' => [
        'title' => 'Create Record',
        'redirect' => 'author/extension/records/edit/{id}',
        'redirectClose' => 'author/extension/records',
    ],
];
```

The following configuration options are available for the Create page:

- `title`: The title of the Create page.
- `redirect`: The URL to redirect to after creating a record.
- `redirectClose`: The URL to redirect to after closing the Create page.
- `flashSave`: The flash message to display after creating a record.
- `configFile`: Override the form definition file for the Create page.

### Edit page

To support the Update page add the following configuration to the form configuration:

```php
public array $formConfig = [
    // ...
    'edit' => [
        'title' => 'Edit Record',
        'redirect' => 'author/extension/records/edit/{id}',
        'redirectClose' => 'author/extension/records',
    ],
];
```

The following configuration options are available for the Update page:

- `title`: The title of the Update page.
- `redirect`: The URL to redirect to after updating a record.
- `redirectClose`: The URL to redirect to after closing the Update page.
- `flashSave`: The flash message to display after updating a record.
- `configFile`: Override the form definition file for the Update page.

### Preview page

To support the Preview page add the following configuration to the form configuration:

```php
public array $formConfig = [
    // ...
    'preview' => [
        'title' => 'Preview Record',
    ],
];
```

The following configuration options are available for the Preview page:

- `title`: The title of the Preview page.
- `configFile`: Override the form definition file for the Preview page.

### Restricting with permissions

You can restrict access to the form controller actions by setting the `$requiredPermissions` property in the controller class. For example, restricting access to the Create page:

```php
class MyController extends \Igniter\Admin\Classes\AdminController
{
    public null|string|array $requiredPermissions = [
        'create' => 'Author.Extension.ManageRecord'
    ];
}
```

## Defining form fields

Form fields are defined in the form definition file using the `fields` key. Fields can be placed either in the outside area or primary tabs. Here is an example of a form controller with fields defined in the form definition file `resources/models/record.php`:

```php
return [
    'form' => [
        // Outside area
        'fields' => [
            'name' => [
                'label' => 'Name',
                'type' => 'text',
                'span' => 'left',
            ],
            'email' => [
                'label' => 'Email',
                'type' => 'text',
                'span' => 'right',
            ],
        ],
        // Primary tabs
        'tabs' => [
            'fields' => [
                // ...
            ],
        ],
    ],
];
```

### Tab options

The following options are available for form tabs:

- `stretch`: Whether the tab should stretch to fit the parent height.
- `defaultTab`: The default tab to assign fields that do not belong to any tab. Default is `General`.

### Field options

The following options are available for form fields (where applicable):

- `label`: The label to display for the field
- `type`: Defines how the field should be rendered (see [Fields types](../extend/forms#available-field-types) below). Default is `text`.
- `span`: Aligns the field. Options are `left`, `right`, `full`. Default is `left`.
- `comment`: A comment to display below the field.
- `commentAbove`: A comment to display above the field.
- `commentHtml`: Allow HTML markup inside the comment. Default is `false`.
- `placeholder`: The placeholder text for the field.
- `default`: The default value for the field.
- `defaultFrom`: References another field to get the default value from.
- `tab`: The tab to assign the field to.
- `readOnly`: Prevents the field from being modified. Default is `false`.
- `disabled`: Prevents the field from being modified and excludes it from the saved data. Default is `false`.
- `hidden`: Hides the field from the form and excludes it from the saved data. Default is `false`.
- `context`: The context in which the field is displayed. Context can also be passed by using an `@` symbol in the field name, for example, `name@update`.
- `trigger`: Use [trigger events](../extend/forms#trigger-events) specify conditions for this field.
- `dependsOn`: Use [field dependencies](../extend/forms#field-dependencies) to update this field based on changes to another field.
- `required`: Places a red asterisk next to the field label. Default is `false`.
- `cssClass`: Additional CSS classes to apply to the field.
- `attributes`: Additional HTML attributes to apply to the field.
- `containerAttributes`: Additional HTML attributes to apply to the field container.
- `permissions`: The [permissions](../extend/permissions) required to view this field.

### Available field types

The following native field types are available for form fields. For more advanced form fields, a [form widget](../extend/forms#form-widgets) can be used instead.

{.grid-3}
- [Text](../extend/forms#text)
- [Number](../extend/forms#number)
- [Money](../extend/forms#money)
- [Currency](../extend/forms#currency)
- [Password](../extend/forms#password)
- [Email](../extend/forms#email)
- [Permalink](../extend/forms#permalink)
- [URL](../extend/forms#url)
- [Textarea](../extend/forms#textarea)
- [Select](../extend/forms#select)
- [Checkbox](../extend/forms#checkbox)
- [Checkbox list](../extend/forms#checkbox-list)
- [Checkbox toggle](../extend/forms#checkbox-toggle)
- [Radio list](../extend/forms#radio-list)
- [Radio toggle](../extend/forms#radio-toggle)
- [Switch](../extend/forms#switch)
- [Section](../extend/forms#section)
- [Partial](../extend/forms#partial)
- [Widget](../extend/forms#widget)

#### Text

`text` renders a simple text input field.

```php
'name' => [
    'label' => 'Name',
    'type' => 'text',
],
```

#### Number

`number` renders an input field that takes only numbers.

```php
'quantity' => [
    'label' => 'Quantity',
    'type' => 'number',
],
```

#### Money

`money` renders an input field that takes only numbers and formats the value to two decimal places.

```php
'price' => [
    'label' => 'Price',
    'type' => 'money',
],
```

#### Currency

`currency` renders an input field that takes only numbers and formats the value matching the active currency settings.

```php
'price' => [
    'label' => 'Price',
    'type' => 'currency',
],
```

#### Password

`password` renders a password input field.

```php
'password' => [
    'label' => 'Password',
    'type' => 'password',
],
```

#### Email

`email` renders an input field that takes only email addresses.

```php
'email' => [
    'label' => 'Email',
    'type' => 'email',
],
```

#### Permalink

`permalink` renders an input field that only accepts alphanumeric characters, dashes, and underscores.

```php
'slug' => [
    'label' => 'Slug',
    'type' => 'permalink',
],
```

#### URL

`url` renders an input field that only accepts valid URLs.

```php
'website' => [
    'label' => 'Website',
    'type' => 'url',
],
```

#### Textarea

`textarea` renders a textarea input field.

```php
'description' => [
    'label' => 'Description',
    'type' => 'textarea',
    
    // Optional
    'attributes' => [
        'rows' => 5,
    ],
],
```

#### Select

`select` renders a dropdown select field. There are 6 ways to provide the dropdown options. All six ways should return an array of options in the format `key => label`.

Provide an array of **options directly** in the field definition.

```php
'order_type' => [
    'label' => 'Order type',
    'type' => 'select',
    'options' => [
        'delivery' => 'Delivery',
        'collection' => 'Pick-up',
    ],  
],
```

Provide an array of options from a **matching method** `get*FieldName*Options` declared in the model class. Using the example above, the model should declare the `getOrderTypeOptions` method.

```php
public function getOrderTypeOptions($value, $formData)
{
    return ['delivery' => 'Delivery', ...];
}
```

Additionally, you can declare a **global method** `getDropdownOptions` in the model class to provide dropdown options for all `select` fields.

```php
public function getDropdownOptions($fieldName, $value, $formData)
{
    if ($fieldName == 'order_type') {
    return ['delivery' => 'Delivery', ...];
    }
    else {
        return ['' => '-- none --'];
    }
}
```

Provide an array of options from a **specific model method** declared in the model class.

```php
'order_type' => [
    // ...
    'type' => 'select',
    'options' => 'listOrderTypes'
],
```

```php
public function listOrderTypes()
{
    return ['delivery' => 'Delivery', ...];
}
```

Provide an array of options from a **static method** declared on a class.

```php
'order_type' => [
    // ...
    'type' => 'select',
    'options' => \Author\Extension\Classes\FormHelper::listOrderTypes
],
```

```php
public static function listOrderTypes($formWidget, $formField)
{
    return ['delivery' => 'Delivery', ...];
}
```

Provide an array of options from a **callable object** via an array definition.

```php
'order_type' => [
    // ...
    'type' => 'select',
    'options' => [Author\Extension\Classes\FormHelper::class, 'listOrderTypes']
],
```

```php
public static function listOrderTypes($formWidget, $formField)
{
    return ['delivery' => 'Delivery', ...];
}
```

The following options are available for the `select` field type:

- `placeholder`: The placeholder text for the field when no option is selected or the field is empty.
- `showSearch`: Whether to enable search functionality in the dropdown. Default is `false`.
- `multiOption`: Whether to allow multiple selections. Default is `false`.

#### Checkbox

`checkbox` renders a checkbox input field.

```php
'is_active' => [
    'label' => 'Is active',
    'type' => 'checkbox',
],
```

#### Checkbox list

`checkboxlist` renders a list of checkboxes. Checkbox lists support the same methods for defining the options as the [select field type](../extend/forms#select).

```php
'colors' => [
    'label' => 'Colors',
    'type' => 'checkboxlist',
    'options' => [
        'red' => 'Red',
        'green' => 'Green',
        'blue' => 'Blue',
    ],
],
```

Options for checkbox lists also support secondary descriptions using the array format `key => [label, description]`.

```php
'colors' => [
    // ...
    'options' => [
        'red' => ['Red', 'This is the color red'],
        'green' => ['Green', 'This is the color green'],
        'blue' => ['Blue', 'This is the color blue'],
    ],
],
```

#### Checkbox toggle

`checkboxtoggle` renders a button-like checkbox toggle. Checkbox toggles support the same methods for defining the options as the [select field type](../extend/forms#select).

```php
'colors' => [
    'label' => 'Colors',
    'type' => 'checkboxtoggle',
    'options' => [
        'red' => 'Red',
        'green' => 'Green',
        'blue' => 'Blue',
    ],
],
```

#### Radio list

`radiolist` renders a list of radio buttons. Radio lists support the same methods for defining the options as the [select field type](../extend/forms#select). These options also support secondary descriptions just like [checkbox lists](../extend/forms#checkbox-list).

```php
'colors' => [
    'label' => 'Colors',
    'type' => 'radiolist',
    'options' => [
        'red' => 'Red',
        'green' => 'Green',
        'blue' => 'Blue',
    ],
],
```

#### Radio toggle

`radiotoggle` renders a button-like radio toggle. Radio toggles support the same methods for defining the options as the [select field type](../extend/forms#select).

```php
'color' => [
    'label' => 'Color',
    'type' => 'radiotoggle',
    'options' => [
        'red' => 'Red',
        'green' => 'Green',
        'blue' => 'Blue',
    ],
],
```

#### Switch

`switch` renders a switch toggle field.

```php
'is_active' => [
    'label' => 'Is active',
    'type' => 'switch',
    'onText' => 'Yes',
    'offText' => 'No',
],
```

#### Section

`section` renders a section heading and subheading.

```php
'section' => [
    'label' => 'Section',
    'type' => 'section',
    'comment' => 'This is a section comment',
],
```

#### Partial

`partial` renders a partial view. The `path` option should be the path to the partial view file otherwise the field name is used as the partial name.

```php
'partial' => [
    'type' => 'partial',
    'path' => 'path/to/partial',
],
```

#### Widget

`widget` renders a form widget. The `type` option should be the class name of the form widget or the registered alias name.

```php
'widget' => [
    'type' => \Igniter\Admin\FormWidgets\RichEditor::class,
],
```

## Form widgets

TastyIgniter includes several form widgets, although it is common for extensions to provide their own custom form widgets. For a deeper dive into how these widgets work and how you can implement or modify them, you can refer to the [Form Widgets](../extend/widgets#using-form-widget) article.

{.grid-2}
- [Code editor](../extend/forms#code-editor)
- [Color picker](../extend/forms#color-picker)
- [Components](../extend/forms#components)
- [Connector](../extend/forms#connector)
- [Data table](../extend/forms#data-table)
- [Date picker](../extend/forms#date-picker)
- [Markdown editor](../extend/forms#markdown-editor)
- [Media finder](../extend/forms#media-finder)
- [Record editor](../extend/forms#record-editor)
- [Relation](../extend/forms#relation)
- [Repeater](../extend/forms#repeater)
- [Rich editor](../extend/forms#rich-editor--wysiwyg)
- [Status editor](../extend/forms#status-editor)
- [Template editor](../extend/forms#template-editor)

#### Code editor

`codeeditor` renders an editor field for editing code or HTML markup.

```php
'code' => [
    'label' => 'Code',
    'type' => 'codeeditor',
],
```

The following options are available for the `codeeditor` form widget type:

- `mode`: The programming language to use for syntax highlighting. Default is `css`.
- `theme`: The color theme option passed to the CodeMirror JS library. Default is `material`.

#### Color picker

`colorpicker` renders the browser supported color picker.

```php
'color' => [
    'label' => 'Color',
    'type' => 'colorpicker',
],
```

The following options are available for the `colorpicker` form widget type:

- `availableColors`: An array of colors to display in the color picker for quick color selection.

#### Components

`components` renders a field for selecting and managing both [theme](../customize/components#theme-component) and [livewire](../customize/components#livewire-component) components from the component library as well as rendering them to a theme template.

```php
'components' => [
    'label' => 'Components',
    'type' => 'components',
    'form' => [
        'fields' => [
            // ...
        ],
    ],
],
```

The following options are available for the `components` form widget type:

- `form`: The form definition for the component fields.
- `prompt`: The prompt text to display when no component is selected.
- `addTitle`: The title of the add button. Default is `Add component`.
- `editTitle`: The title of the edit button. Default is `Edit component`.
- `copyPartialTitle`: The title of the override partial button. Default is `Override component partial`.

#### Connector

`connector` renders a field for attaching and managing related records from a model.

```php
'products' => [
    'label' => 'Products',
    'type' => 'connector',
    'form' => [
        'fields' => [
            'name' => [
                'label' => 'Name',
                'type' => 'text',
            ],
            'price' => [
                'label' => 'Price',
                'type' => 'money',
            ],
        ],
    ],
],
```

The following options are available for the `connector` form widget type:

- `form`: The form definition for creating and managing related records. Either a reference to the [form definition file](../extend/forms#form-definition-file) or an array of fields.
- `formName`: The form name to use for the record editor popup. Default is `Record`.
- `nameFrom`: The model attribute to use as the display name for the related records. Default is `name`.
- `descriptionFrom`: The model attribute to use as the description for the related records. Default is `description`.
- `partial`: The partial view to use for rendering each related record. Leave empty to use the default partial.
- `sortable`: Whether to allow sorting of the related records. Default is `false`.
- `sortColumnName`: The field name to use for sorting the related records. Default is `priority`.
- `editable`: Whether to allow editing of the related records. Default is `true`.
- `newRecordTitle`: The title of the popup when creating a record . Default is `New %s`.
- `editRecordTitle`: The title of the popup when editing a record. Default is `Edit %s`.
- `emptyMessage`: The message to display when there are no related records. For example: `No records found`.
- `confirmMessage`: The message to display when deleting a record. For example: `Are you sure you want to delete this record?`.
- `popupSize`: The size of the popup window. Options are `modal-sm`, `modal-md`, `modal-lg`, `modal-xl`.
- `hideNewButton`: Whether to hide the new related record button. Default is `true`.

#### Data table

`datatable` renders a table for displaying and editing tabular data.

```php
'products' => [
    'label' => 'Products',
    'type' => 'datatable',
    'columns' => [
        'name' => [
            'title' => 'Name',
        ],
        'price' => [
            'title' => 'Price',
        ],
    ],
],
```

The following options are available for the `datatable` form widget type:

- `columns`: An array of columns to display in the table. Each column should have a `title` definition.
- `defaultSort`: The default column to sort by. Example `name asc`.
- `searchableFields`: The fields to search when filtering the table.
- `showRefreshButton`: Whether to show the refresh button. Default is `true`.
- `useAjax`: Whether to use AJAX to load the table data. Default is `false`.
- `showPagination`: Whether to show pagination controls. Default is `true`.
- `pageLimit`: The number of records to display per page. Default is `10`.
- `dynamicHeight`: Whether to adjust the table height based on the number of records. Default is `false`.
- `height`: The height of the table. Default is `auto`.
- `keyFrom`: The model attribute to use as the primary key. Default is `id`.
- `toolbar`: The configuration of toolbar buttons to display above the table.
- `fieldName`: The custom field name to use for the table. Default to the field alias.

#### Date picker

`datepicker` renders the browser supported date picker field for selecting dates and times.

```php
'date' => [
    'label' => 'Date',
    'type' => 'datepicker',
],
```

The following options are available for the `datepicker` form widget type:

- `mode`: The date picker mode. Options are `date`, `time`, `datetime`. Default is `date`.
- `dateFormat`: The date format to use when displaying the date. Default is `Y-m-d`.
- `timeFormat`: The time format to use when displaying the time. Default is `H:i`.
- `startDate`: The minimum date that can be selected.
- `endDate`: The maximum date that can be selected.

#### Markdown editor

`markdown` renders an editor field for editing markdown content.

```php
'markdown' => [
    'label' => 'Markdown',
    'type' => 'markdown',
],
```

The following options are available for the `markdown` form widget type:

- `mode`: The editor view mode. Options are `tab` or `split`. Default is `tab`.

#### Media finder

`mediafinder` renders a field for selecting media files from the media library.

```php
'image' => [
    'label' => 'Image',
    'type' => 'mediafinder',
],
```

The following options are available for the `mediafinder` form widget type:

- `mode`: The media finder view mode. Options are `grid`, `inline`. Default is `grid`.
- `prompt`: The prompt text to display when no media is selected.
- `isMulti`: Whether to allow multiple media selections. Default is `false`.
- `useAttachment`: Whether to use the attachment ID instead of the media path. Default is `false`.
- `thumbOptions`: Configuration options for generating media thumbnails. Default is `['fit' => 'contain', 'width' => 122, 'height' => 122]`.

##### Record editor

`recordeditor` renders a field for selecting and managing records from a model.

```php
'products' => [
    'label' => 'Products',
    'type' => 'recordeditor',
    'modelClass' => 'Author\Extension\Models\Product',
    'form' => [
        'fields' => [
            'name' => [
                'label' => 'Name',
                'type' => 'text',
            ],
            'price' => [
                'label' => 'Price',
                'type' => 'money',
            ],
        ],
    ],
],
```

The following options are available for the `recordeditor` form widget type:

- `modelClass`: The model class to use for fetching records.
- `form`: The form definition for creating and managing records. Either a reference to the [form definition file](../extend/forms#form-definition-file) or an array of fields.
- `formName`: The form name to use for the record editor popup. Default is `Record`.
- `addonRight`: An array of button options (see below) to display on the right side of the record editor.
- `addonLeft`: An array of button options (see below) to display on the left side of the record editor.
- `popupSize`: The size of the popup window. Options are `modal-sm`, `modal-md`, `modal-lg`, `modal-xl`.
- `hideEditButton`: Whether to hide the edit button. Default is `false`.
- `hideDeleteButton`: Whether to hide the delete button. Default is `false`.
- `hideCreateButton`: Whether to hide the create button. Default is `false`.
- `attachToField`: The field name to attach the selected record to.
- `addLabel`: The label to display on the create button. Default is `Add`.
- `editLabel`: The label to display on the edit button. Default is `Edit`.
- `deleteLabel`: The label to display on the delete button. Default is `Delete`.
- `attachLabel`: The label to display on the attach button. Default is `Attach`.

The following options are available for the `addonRight` and `addonLeft` buttons:

- `label`: The button label.
- `tag`: The button tag. Options are `div`, `button`, `a`. Default is `div`.
- `attributes`: Additional HTML attributes to apply to the button. Define as an array of key-value pairs.

#### Relation

`relation` renders a single or multiple selection dropdown field for selecting related records. The label used for displaying each relation is retrieved by the `nameFrom` or `select` definition.

```php
'category_id' => [
    'label' => 'Category',
    'type' => 'relation',
    'relationFrom' => 'category',
    'nameFrom' => 'name',
],
```

The following options are available for the `relation` form widget type:

- `relationFrom`: The relation name to use for fetching related records, if different from the field name.
- `nameFrom`: The model attribute name to use for displaying the related label.
- `select`: A custom SQL select statement to use for each relation item label.
- `order`: Used to sort options. Example `name asc`.
- `scope`: A custom query scope method to apply to the relation query.
- `emptyOption`: The text to display for the empty option. Default is `-- none --`.

#### Repeater

`repeater` renders a field for repeating a set of fields.

```php
'products' => [
    'label' => 'Products',
    'type' => 'repeater',
    'form' => [
        'fields' => [
            'name' => [
                'label' => 'Name',
                'type' => 'text',
            ],
            'price' => [
                'label' => 'Price',
                'type' => 'money',
            ],
        ],
    ],
],
```

The following options are available for the `repeater` form widget type:

- `form`: The form definition for the fields to repeat. Either a reference to the [form definition file](../extend/forms#form-definition-file) or an array of fields.
- `prompt`: The text to display for the create button. Default is `Add item`.
- `sortable`: Whether to allow sorting of the repeater items. Default is `false`.
- `sortColumnName`: The column name to use for sorting the repeater items. Default is `priority`.
- `showAddButton`: Whether to show the add button. Default is `true`.
- `showRemoveButton`: Whether to show the remove button. Default is `true`.
- `emptyMessage`: The message to display when there are no items in the repeater. Default is `lang:igniter::admin.text_empty`.

#### Rich editor / WYSIWYG

`richeditor` renders a rich text editor field for editing HTML content.

```php
'content' => [
    'label' => 'Content',
    'type' => 'richeditor',
],
```

The following options are available for the `richeditor` form widget type:

- `size`: The size of the editor. Options are `small`, `large`. Default is `large`.
- `toolbarButtons`: The buttons to display in the editor toolbar.

#### Status editor

`statuseditor` renders a field for updating status and assignee for orders and reservations.

```php
'status' => [
    'label' => 'Status',
    'type' => 'statuseditor',
    'form' => [
        'fields' => [
            'status_id' => [
                'label' => 'Status',
                'type' => 'select',
                'options' => 'listStatuses',
            ],
            'assignee_id' => [
                'label' => 'Assignee',
                'type' => 'select',
                'options' => 'listAssignees',
            ],
        ],
    ],
],
```

The following options are available for the `statuseditor` form widget type:

- `form`: The form definition for creating and managing status and assignee. Either a reference to the [form definition file](../extend/forms#form-definition-file) or an array of fields.
- `formTitle`: The title of the popup window. Default is `Add %s`.
- `statusFormName`: The form name to use for the status editor popup.
- `assigneeFormName`: The form name to use for the assignee editor popup.
- `statusKeyFrom`: The model attribute to use as the primary key for the status.
- `assigneeKeyFrom`: The model attribute to use as the primary key for the assignee.
- `assigneeGroupKeyFrom`: The model attribute to use as the primary key for the assignee group.
- `statusNameFrom`: The model attribute to use as the display name for the status.
- `assigneeNameFrom`: The model attribute to use as the display name for the assignee.
- `assigneeGroupNameFrom`: The model attribute to use as the display name for the assignee group.
- `statusColorFrom`: The model attribute to use as the color for the status.
- `statusRelationFrom`: The relation name to use for fetching status records.
- `assigneeRelationFrom`: The relation name to use for fetching assignee records.

#### Template editor

`templateeditor` renders a field for editing theme template files.

```php
'content' => [
    'label' => 'Content',
    'type' => 'templateeditor',
],
```

The following options are available for the `templateeditor` field type:

- `form`: The form definition for creating and managing [layouts](../customize/layouts) and [pages](../customize/pages) template files. Either a reference to the [form definition file](../extend/forms#form-definition-file) or an array of fields.
- `formName`: The form name to use for the template editor popup.
- `addLabel`: The label to display on the add button.
- `editLabel`: The label to display on the edit button.
- `deleteLabel`: The label to display on the delete button.
- `placeholder`: The placeholder text to display when no template is selected.

## Displaying the form

TastyIgniter provides a view file with the corresponding name (`create.blade.php`, `edit.blade.php` and `preview.blade.php`) for each page supported by form controllers, so you don't have to create them manually.

You can use the `renderForm` method on the `FormController` class to render the form fields. The first argument is an array of options to pass to the form fields, and the second argument is a boolean value indicating whether to render the toolbar buttons.

### Create page

Here is an example of what the `create.blade.php` blade view file might look like:

```blade
{!! form_open([
    'id'     => 'edit-form',
    'role'   => 'form',
    'method' => 'POST',
]) !!}

    {!! $this->renderFormToolbar() !!}

    {!! $this->renderForm([], true) !!}

{!! form_close() !!}
```

In the example above, the `renderForm` method is used to render the form fields. The first argument is an array of options to pass to the form fields, and the second argument is a boolean value indicating whether to render the toolbar buttons.

### Edit page

Here is an example of what the `edit.blade.php` blade view file might look like:

```blade
{!! form_open([
    'id'     => 'edit-form',
    'role'   => 'form',
    'method' => 'PATCH',
]) !!}

    {!! $this->renderFormToolbar() !!}

    {!! $this->renderForm([], true) !!}

{!! form_close() !!}
```

> Notice the `PATCH` method used in the form open tag.

### Preview page

Here is an example of what the `preview.blade.php` blade view file might look like:

```blade
{!! form_open([
    'id'     => 'edit-form',
    'role'   => 'form',
    'method' => 'POST',
]) !!}

    {!! $this->renderFormToolbar() !!}

    {!! $this->renderForm(['preview' => true], true) !!}

{!! form_close() !!}
```

## Applying conditions to fields

Sometimes you want to manipulate the value or visibility of a form field based on specific rules, such as hiding an input if a checkbox is selected. There are few ways to achieve this, including using the trigger API or field dependencies. These options are discussed in more detail below.

### Input preset converter

The input preset converter is a way to convert the value of a field based of the value in another field. You can use the `preset` property in the field definition to define `preset` options. For example, to convert a value on the `title` field to a _slug_ on the `permalink` field:

```php
'title' => [
    'label' => 'Title',
    'type' => 'text',
],
'permalink' => [
    'label' => 'Permalink',
    'type' => 'permalink',
    'preset' => [
        'field' => 'title',
        'type' => 'slug',
    ],
],
```

The following options are available for the `preset` property:

- `field`: The name of the other field that this field get its value from.
- `type`: The type of preset to apply. Options are `exact`, `slug`, `url`, `camelCase`, and `file`.
- `prefixInput`: The CSS Selector of the input element to get the prefix value from.

### Trigger events

In TastyIgniter, you can apply conditions to form fields using the `trigger` property in the field definition. This allows you to create dynamic forms where the visibility or value of a field can change based on the state of other fields. For example, to show a field only when a checkbox is selected:

```php
'is_active' => [
    'label' => 'Is active',
    'type' => 'checkbox',
],
'activation_date' => [
    'label' => 'Activation Date',
    'type' => 'datepicker',
    'trigger' => [
        'action' => 'show',
        'field' => 'is_active',
        'condition' => 'checked',
    ],
],
```

In this example, the `activation_date` field will only be shown if the `is_active` checkbox is checked. The trigger property is an array that defines the following keys:

- `action`: The action to perform. It can be `show`, `hide`, `enable`, `disable`.
- `field`: The name of the other field that this field is dependent on.
- `condition`: The condition that must be met for the action to be performed. It can be `checked`, `unchecked`, `value[somevalue]`.

### Field dependencies

Field dependencies are a way to update a field based on changes to another field. You can use the `dependsOn` property in the field definition to define the field dependencies. The `dependsOn` property is a string that defines the name of the other field that this field is dependent on. For example:

```php
'is_active' => [
    'label' => 'Is active',
    'type' => 'checkbox',
],
'activation_date' => [
    'label' => 'Activation Date',
    'type' => 'datepicker',
    'dependsOn' => 'is_active',
],
```

In this example, the `activation_date` field will be updated whenever the `is_active` checkbox is changed.

### Preventing a field from being submitted

In TastyIgniter, you can prevent a field from being submitted by prefixing the field name with an underscore `_`. Form fields beginning with underscore will be excluded from the saved data. For example:

```php
'_map' => [
    'label' => 'Point your address on the map',
    'type' => 'mapview',
],
```

Additionally, you can add the `disabled` property to the field definition to prevent the field from being modified and exclude it from the saved data. For example:

```php
'map' => [
    'label' => 'Point your address on the map',
    'type' => 'mapview',
    'disabled' => true,
],
```

## Validating form fields

To validate the fields of your form you should add a form request class to the `request` key in the form controller configuration `$formConfig` property. The form request class should extend the `Igniter\System\Classes\FormRequest` class and define a `rules` method that returns an array of validation rules. See [Validation](../advanced/validation) for more information on how to define validation rules.

## Extending the form controller

You may need to extend the form controller to add custom functionality. There are several ways to extend the form controller:

### Overriding controller action

You can override the `create`, `edit` and `preview` controller action by defining a method with the same name in your controller, then optionally call the parent method to execute the default action. For example:

```php
public function edit($recordId, $context = null)
{
    // Your custom logic here

    $this->asExtension('FormController')->edit($recordId, $context);
}
```

### Overriding form views

You can override the form views by creating a new view file in the `resources/views` directory of an extension. The view file should have the same name as the form view file you want to override. For example, to override the `create.blade.php` view file rendered from `MyController`, create a new view file in the `resources/views/mycontroller` directory with the same name. Here is an example adding a sidebar to the form **create** page:

```blade
<div class="row">
    <div class="col-md-8">
        {!! form_open([
            'id'     => 'edit-form',
            'role'   => 'form',
            'method' => 'POST',
        ]) !!}
            {!! $this->renderFormToolbar() !!}
            {!! $this->renderForm([], true) !!}
        {!! form_close() !!}
    </div>
    <div class="col-md-4">
        <div class="card">
            <div class="card-body">
                <h5 class="card-title">Sidebar</h5>
                <p class="card-text">This is a sidebar</p>
            </div>
        </div>
    </div>
</div>
```

### Extending form model query

You can extend the model query by overriding the `formExtendQuery` method in your controller. This method is called before the form fields are rendered and can be used to modify the query used to fetch the model data. For example:

```php
public function formExtendQuery($query)
{
    return $query->where('status', 'active');
}
```

Or, you can use the `admin.controller.extendFormQuery` event to extend the query from another extension. For example:

```php
Event::listen('admin.form.extendQuery', function ($controller, $query) {
    if ($controller instanceof MyController) {
        $query->where('status', 'active');
    }
});
```

### Extending form fields

You can extend the form fields by overriding the `formExtendFields` method in your controller. This method is called before the form fields are rendered and can be used to add or modify form fields. For example:

```php
public function formExtendFields(Form $widget)
{
    $widget->addFields([
        'is_active' => [
            'label' => 'Is active',
            'type' => 'switch',
        ],
    ]);
}
```

Or, you can use the `admin.form.extendFields` event to extend the form fields from another extension. For example:

```php
Event::listen('admin.form.extendFields', function (Form $widget, $fields) {
    if ($widget->getController() instanceof MyController) {
        $widget->addFields([
            'is_active' => [
                'label' => 'Is active',
                'type' => 'switch',
            ],
        ]);
    }
});
```

Additionally, you can override the `formExtendFieldsBefore` method in your controller or use the `admin.form.extendFieldsBefore` event to modify the query before the form fields are rendered. For example:

```php
public function formExtendFieldsBefore(Form $widget)
{
    unset($widget->fields['name']);
}

// Or use the event

Event::listen('admin.form.extendFieldsBefore', function (Form $widget, $query) {
    if ($widget->getController() instanceof MyController) {
        unset($widget->fields['name']);
    }
});
```

### Filtering form fields

You may filter the form field definitions by overriding the `filterFields` method of the Model used. This lets you change the visibility and other field properties based on model data. The method accepts three parameters. `$widget` form widget object, `$fields` an array containing the fields defined by the field configuration, and `$context` the active form context.

```php
public function filterFields($widget, &$fields, $context = null)
{
    if ($this->source_type == 'http') {
        $fields['source_url']['hidden'] = true;
    }
    
    if ($this->source_type == 'file') {
        $fields['source_file']['hidden'] = true;
    }
}
```

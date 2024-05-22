---
title: "Extensions"
section: extend
sortOrder: 200
---

## Introduction

Extensions are the foundation for adding new features to TastyIgniter by extending it. The core of TastyIgniter is designed to be lean and lightweight, to maximize flexibility and minimize code bloat. With extensions, you can add specific set of features or services, such as: 

- Define components, mail templates and staff permissions
- Add, remove or replace navigation items
- Create or alter database table structures and seed database records
- Alter core or other extension functionality
- Provide services, admin controllers and assets.

TastyIgniter extensions are built on the foundation of Laravel packages. They leverage standard practices such as implementing service providers to register routes, views, and translations. If you're new to Laravel package development, [the Package Development part of the Laravel documentation](https://laravel.com/docs/packages) is a useful resource.

### Directory structure

Below is an example of an extension directory structure. 

```yaml
acme/                     <=== Author name (namespace)
  helloworld/             <=== Extension name
    src/
      Extension.php       <=== Extension class
    composer.json         <=== Contains extension metadata
    README.md             <=== Extension readme file
```

## Creating an extension

This section of the article covers the steps you need to take - and some things you need to consider - when creating a well-structured TastyIgniter extension.

### Naming your extension

The first step in creating an extension is to select a **Namespace** and **Short Name** for it. This extension name **Namespace.ShortName** is used to refer to your extension by core TastyIgniter. The namespace will be used as your author
code when publishing your extensions on the [TastyIgniter marketplace](https://tastyigniter.com/marketplace).

Both namespace and extension name must follow these important rules:

- Only letters must be provided. 
- Folder names must be lowercase, as shown in the directory structure example.
- Should not contain spaces.
- Must be unique. The name of your extension should not be the same with any other extension or theme.

Here's an example: **Acme.HelloWorld**

Given the above example **Acme.HelloWorld**, creating an extension is pretty simple and straightforward, simply call the
following command from the application directory to generate a basic extension directory and files within your `extensions` directory:

```bash
php artisan make:igniter-extension Acme.HelloWorld
```

> We strongly recommend that you follow the [TastyIgniter coding standards](../resources/php-coding-guidelines) when creating your own extensions. It is a requirement for any changes to the TastyIgniter core code.

### Extension manifest (composer.json)

A `composer.json` file is essential for storing metadata about the extension.

```json
{
  "name": "acme/ti-ext-helloworld",
  "type": "tastyigniter-package",
  "description": "Say hello to the rest of the world..",
  "authors": [
    {
      "name": "Acme Labs"
    }
  ],
  "require": {
    "tastyigniter/ti-ext-local": "*"
  },
  "extra": {
    "tastyigniter-extension": {
      "code": "acme.helloworld",
      "name": "Hello World",
      "icon": {
        "class": "fa fa-puzzle-piece",
        "color": "#FFF",
        "backgroundColor": "#ED561A"
      }
    }
  }
}
```

| Field                                     | Description                                                                                                                                                                                                                                                                                    |
|-------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **name**                                  | The Composer package's name in vendor/package format, **required**. You should use a vendor name that is unique to you, such as your GitHub username. You should prefix the package part with <code><b>ti-ext-</b></code> to indicate that your package is intended for use with TastyIgniter. |
| **type**                                  | MUST be set to <code><b>tastyigniter-package</b></code>, ensures that your extension will be installed as such when someone "requires" it, **required**.                                                                                                                                       |
| **description**                           | A one-sentence summary of what the extension does, **required**. (max. char: 130)                                                                                                                                                                                                              |
| **authors**                               | An object to specify the name of the extension author, **required**.                                                                                                                                                                                                                           |
| **require**                               | Defines other TastyIgniter extensions your extension depends on, **optional**. In the example above, **acme.helloworld** extension depends on the **tastyigniter/ti-ext-local** extension.                                                                                                     |
| **extra.tastyigniter-extension**          | Holds TastyIgniter-specific extension metadata, such as your extension's display name and icon style, **required**.                                                                                                                                                                            |
| **extra.tastyigniter-extension.code**     | The extension unique identifier code, **required**.                                                                                                                                                                                                                                            |
| **extra.tastyigniter-extension.name**     | Specifies the extension name, **required**. The value is used as the extension display name.                                                                                                                                                                                                   |
| **extra.tastyigniter-extension.icon**     | An object that defines the icon for your extension. The **name** property is the name of a [Font Awesome icon class](https://fontawesome.com/icons). All other properties are used as the style attribute for your extension's icon.                                                           |
| **extra.tastyigniter-extension.homepage** | Specifies the extension website URL, **optional**.                                                                                                                                                                                                                                             |

See [the composer.json schema](https://getcomposer.org/doc/04-schema.md) documentation for information about other
properties you can add to composer.json.

### Readme file

If you want to publish your extension on the [TastyIgniter marketplace](https://tastyigniter.com/marketplace), you must
create a **README.md** file in a standardized format in your extension directory.

## Extension class

An **Extension.php** file (aka *Extension class*) is an essential part of the TastyIgniter extension for providing methods for extending the TastyIgniter core. 

The extension class should extend the `\Igniter\System\Classes\BaseExtension` class, which in turn extends Laravel's `Illuminate\Foundation\Support\Providers\EventServiceProvider` class. This allows the Extension class to take advantage of the functionality provided by Laravel's service providers, such as registering services, event subscribers, model observers and listening for events.

Here is an example of an Extension class:

```php
namespace Acme\HelloWord;

class Extension extends \Igniter\System\Classes\BaseExtension
{
    public function register()
    {
        
    }

    public function boot()
    {
        
    }
}
```

In this example, the `register` method is used to bind services into the container, and the `boot` method is used to perform any operations after all other services have been registered.

### Extension class methods

The following methods are supported in the extension class:

| Method                         | Description                                                                                                                |
|--------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| **register()**                 | register method, called when the plugin is first registered.                                                               |
| **boot()**                     | boot method, called right before the request route.                                                                        |
| **registerNavigation()**       | registers admin navigation menu items for this extension, see below for example.                                           |
| **registerPermissions()**      | registers any [staff permissions](../extend/permissions#registering-permissions) supplied by this extension.               |
| **registerSettings()**         | registers any [admin settings page](../extend/extensions#registering-settings-navigation-link) supplied by this extension. |
| **registerDashboardWidgets()** | registers any [admin dashboard widgets](../extend/widgets#using-dashboard-widget), supplied by this extension.             |
| **registerFormWidgets()**      | registers any [admin form widgets](../extend/widgets#using-form-widget) supplied by this extension.                        |
| **registerMailLayouts()**      | registers any mail view layouts supplied by this extension.                                                                |
| **registerMailTemplates()**    | registers any mail view templates supplied by this extension, see below for example.                                       |
| **registerMailPartials()**     | registers any mail view partials supplied by this extension.                                                               |
| **registerSchedule()**         | registers any [scheduled tasks](../advanced/scheduling-tasks) supplied by this extension.                                  |

An example registering **mail templates view** file `extensions/acme/helloworld/views/mail/contact`:

```php
    public function registerMailTemplates(): array
    {
        return [
            'acme.helloworld::mail.contact' => 'Contact form email to admin',
        ];
    }

```

An example registering a top-level and child **menu navigation menu item**:

```php
public function registerNavigation(): array
{
    return [
        'messages' => [
            'priority' => 300,
            'title' => 'Messages',
            'href' => admin_url('acme/helloworld/messages'),
            'permission' => ['Acme.HelloWorld'],
            'child' => [
                'banners' => [
                    'priority' => 500,
                    'title' => 'Banners',
                    'href' => admin_url('acme/helloworld/banners'),
                    'permission' => ['Acme.HelloWorld.Banners'],
                ],
            ],
        ],
    ];
}
```

To register a custom **middleware**, you can call any of the following in your **boot** method.

```php
public function boot()
{
    // Add a new middleware to a controller class.
   \Igniter\Main\Classes\MainController::extend(function($controller) {
        $controller->middleware(\Path\To\CustomMiddleware::class);
    });
    
    // Alternatively, add a new middleware to beginning of the igniter group stack.
    $this->app['router']->prependMiddlewareToGroup('igniter', \Path\To\CustomMiddleware::class);

    // Alternatively, add a new middleware to end of the igniter group stack.
    $this->app['router']->pushMiddlewareToGroup('igniter', \Path\To\CustomMiddleware::class);
}
```

An example registering **console commands**:

```php
public function register()
{
    $this->registerConsoleCommand(
        'create.apiresource', \Igniter\Api\Console\CreateApiResource::class
    );
}
```

## Resources

### Settings & Configuration

There are two ways to configure extensions; with settings models (database settings) and configuration files.

#### Using the settings model configuration

Settings models, like any other models, resides in the **src/Models** subdirectory of the extension directory.

```yaml
acme/
  helloworld/
    resources/
      models/
        settings.php    	<=== Setting model form fields
    src/
      Models/
        Settings.php     	<=== Setting model class file
```

By implementing the `\Igniter\System\Actions\SettingsModel` action class and extending the base `\Igniter\Flame\Database\Model` class in a model class, you can create models for storing settings in the `extension_settings` database table.

```php
<?php namespace Acme\HelloWorld\Models;

class Settings extends \Igniter\Flame\Database\Model
{
    public array $implement = [\Igniter\System\Actions\SettingsModel::class];

    // A unique code used for saving the settings to the database
    public string $settingsCode = 'acme_helloworld_settings';

    // Reference to form field model config file, without the .php extension. 
    // Should match the model class name
    public string $settingsFieldsConfig = 'settings';
}
```

The settings model form fields are defined in a `settings.php` PHP file in the `resources/models` directory. The file should return an array of form field definitions.

```php
<?php

return [
    'form' => [
        'toolbar' => [
            'buttons' => [
                'save' => ['label' => 'lang:admin::lang.button_save', 'class' => 'btn btn-primary', 'data-request' => 'onSave'],
                'saveClose' => [
                    'label' => 'lang:admin::lang.button_save_close',
                    'class' => 'btn btn-default',
                    'data-request' => 'onSave',
                    'data-request-data' => 'close:1',
                ],
            ],
        ],
        'fields' => [
            'field_name' => [
                'label' => 'Field Label',
                'type' => 'text',
                'span' => 'left',
            ],
        ],
    ],
];
```

An example of **writing** to a settings model:

```php
use Acme\HelloWorld\Models\Settings;

// Set a single value
Settings::set('cart_key', 'ABCD');

// Set an array of values
Settings::set(['cart_key' => 'ABCD']);
```

An example of **reading** from a settings model:

```php
use Acme\HelloWorld\Models\Settings;

// Get a single value
$cartKey = Settings::get('cart_key');

// Get a value and return a default value if it doesn't exist
$abandonedCart = Settings::get('abandoned_cart', true);
```

#### Using the file-based configuration

Just like Laravel packages, extensions can have configuration files in the extension `config` subdirectory. Configuration files are PHP scripts that define and return an array, e.g. `acme/helloworld/config/settings.php` configuration file

```php
<?php

return [
    'destroyOnLogout' => true,
    'cartSessionTtl' => 120
];
```
To make the configuration file available to the application, you need to register the configuration file in the extension class `register` method.

```php
public function register()
{
    $this->mergeConfigFrom(__DIR__.'/config/settings.php', 'acme.helloworld::settings');
}
```

Use the `Config` facade to access the configuration values defined in the configuration file.

```php
use Config;

// Get a value and return a default value if it doesn't exist
$cartSessionTtl = Config::get('acme.helloworld::settings.cartSessionTtl', 120);
```

#### Registering settings navigation link

An example showing how to register a system settings item which links to a settings model. Registered settings will appear on the _Manage > Settings_ admin page.

```php
public function registerSettings(): array
{
    return [
        'settings' => [
            'label' => 'Extension Settings',
            'description' => 'Manage extension settings.',
            'icon' => 'fa fa-plugin',
            'model' => Acme\HelloWorld\Models\Settings::class,
            'permissions' => ['Acme.HelloWorld.Settings'],
        ],
    ];
}
```

### Migrations

If your extension contains database migrations stored in the extension's `database/migrations` directory, TastyIgniter will automatically load them when the extension is loaded.

Here is an example of a `2020_01_01_000000_create_messages_table.php` migration file located in `acme/helloworld/database/migrations`:

```php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up()
    {
        Schema::create('acme_helloworld_messages', function (Blueprint $table) {
            $table->increments('id');
            $table->string('subject');
            $table->text('message');
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('acme_helloworld_messages');
    }
};
```

For more information on database migrations, refer to the [Laravel documentation on database migrations](https://laravel.com/docs/migrations).

### Seeders

Seeders are used to populate database tables with sample data. Just like Laravel Packages, seeders are stored in the extension's `database/seeds` directory. 

Here is an example of a `MessagesTableSeeder.php` seeder file located in `acme/helloworld/database/seeds`:

```php
namespace Acme\HelloWorld\Database\Seeds;

use Illuminate\Database\Seeder;

class MessagesTableSeeder extends Seeder
{
    public function run()
    {
        DB::table('acme_helloworld_messages')->insert([
            'subject' => 'Hello World',
            'message' => 'Welcome to the world of TastyIgniter!',
        ]);
    }
}
```

To run the seeder, you can use the `php artisan db:seed --class=Acme\\HelloWorld\\Database\\Seeds\\MessagesTableSeeder` command.

### Routes

If your extension includes a `routes/web.php` route file, TastyIgniter will automatically load it, adding any defined routes to the application's routing table. 

Extensions in TastyIgniter can define routes using the `routes/web.php` file or handle admin routes through admin controllers.

To handle admin routes, use [admin controllers](../extend/controllers). TastyIgniter automatically maps admin URLs to any controller class that extends the `Igniter\Admin\Classes\AdminController` and resides in the extension's `src/Http/Controllers` directory. For example, admin URL `acme/helloworld/messages` will be mapped to the `Acme\HelloWorld\Http\Controllers\Messages` admin controller.

### Language files

If your extension contains [language files](../advanced/localization) stored in the extension's `resources/lang` directory, TastyIgniter will automatically load them when the extension is loaded. When developing your extension, you should consider using language strings even if you do not intend to use it in more than one language. You never know: if you decide to make your extension available to users around the world, it can be handy later.

It's highly recommended to include a complete set of English resources with your extension, especially if you plan to publish your extension in the TastyIgniter marketplace.

Here's an example of a language file named `default.php` located in `acme/helloworld/resources/lang`:

```php
return [
    'label' => 'Hello World',
    'description' => 'Say hello to the rest of the world.',
];
```

To reference these language lines, use the `author.extension::file.key` syntax. For instance, to access the `label` language string from the `acme.helloworld` extension, use the `lang` helper function:

```php
echo lang('acme.helloworld::default.label');
```

In the example above, `acme.helloworld` prefix is the extension code, `default` is the language file name without the `.php` extension, and `label` is the key of the language string.

### Views

If your extension contains views stored in the extension's `resources/views` directory, TastyIgniter will automatically load them when the extension is loaded.

Extension views are referenced using the `author.extension::view` syntax convention. So, once your extension is loaded, you can load the `hello.blade.php` view file from the `acme.helloworld` extension like so:

```php
return view('acme.helloworld::hello');
```

#### Overriding extension views

When TastyIgniter automatically loads your view files, it registers two locations for your views: the application's `resources/views/vendor` directory and the extension's own `resources/views` directory. This setup enables developers to customize or override the views of your extension by placing a modified version in the `resources/views/vendor/author/extension` directory within the application.

For more information on views, refer to the [Laravel documentation on views](https://laravel.com/docs/views).

## Using Third-Party composer packages

There are a number of scenarios that require the developer to add a third-party composer packages to their extension.

Here is a full example of how the **Igniter.Api** extension adds third-party package `laravel/sanctum`:

```json
{
  "name": "tastyigniter/ti-ext-api",
  "type": "tastyigniter-package",
  "require": {
    "laravel/sanctum": "^3.0"
  },
  "extra": {
    "tastyigniter-extension": {
      "code": "igniter.api",
      "name": "APIs",
      "icon": {
        "class": "fa fa-cloud",
        "color": "#fff",
        "backgroundColor": "#02586F"
      },
      "homepage": "https://tastyigniter.com/marketplace/item/igniter-api"
    }
  }
}
```

> Do not commit the `/vendor` directory, the `composer.json` file should be committed to the repository.

## Extending an extension

Why extend one extension with another? To ensure that the changes remain compatible if the extension is updated. Extensions are mostly used to inject or modify the functionality of core classes and other extensions via the 'Event' dispatcher service.

The most common place to subscribe to an event is the extension class `boot` method.

---
title: "Extensions"
section: extend
sortOrder: 200
---

## Introduction

Extensions are the foundation for adding new features to TastyIgniter by extending it. The core of TastyIgniter is designed to be lean and lightweight, to maximize flexibility and minimize code bloat. With extensions, you can add functionality to your site, instead of hacking the core code of TastyIgniter.

An extension can add to TastyIgniter specific set of features or services, such as: define components, defines staff permissions, add or extend menu items, lists and forms, create or alter database table structures and seed data, alter core or other extension functionality, provide services, admin controllers, views, assets and other files.

### How TastyIgniter Loads Extensions

When TastyIgniter loads the list of extensions on the **System -> Extensions** page of the TastyIgniter Admin, it searches through the `extensions` folder (and its second level subdirectories) to find the `Extension.php` PHP Class file. 

### Directory structure

Extensions live in the **/extensions** subdirectory of the application directory. Below is an example of an extension directory structure. 

```yaml
extensions/
acme/           		    <=== Author name (namespace)
helloworld/         	<=== Extension name
      components/
      controllers/
models/
composer.json		    <=== Contains extension metadata
Extension.php			<=== Extension registration file
      README.md				<=== Extension readme file
      routes.php			<=== Extension routes file
```

## Creating an extension

This section of the article covers the steps you need to take – and some things you need to consider – when creating a well-structured TastyIgniter extension.

### Naming your extension

The first step in creating an extension is to select a **Namespace** and **Short Name** for it. This extension name **
Namespace.ShortName** is used to refer to your extension by core TastyIgniter. The namespace will be used as your author
code when publishing your extensions on the [TastyIgniter marketplace](https://tastyigniter.com/marketplace).

Both namespace and extension name must follow these important rules:

- Only letters must be provided. 
- Folder names must be lowercase, as shown in the directory structure example.
- Should not contain spaces.
- Must be unique. The name of your extension should not be the same with any other extension or theme.

Here's an example: **Acme.HelloWorld**

Given the above example **Acme.HelloWorld**, creating an extension is pretty simple and straightforward, simply call the
following command from the application directory to generate a basic extension directory and files:

```bash
php artisan create:extension Igniter.HelloWorld
```

> We strongly recommend that you follow the [TastyIgniter coding standards](../advanced/php-coding-guidelines) when creating your own extensions. It is a requirement for any changes to the TastyIgniter core code.

### Extension manifest file (composer.json)

A `composer.json` file is essential for storing metadata about the extension.

```json
{
  "name": "acme/ti-ext-helloworld",
  "type": "tastyigniter-extension",
  "description": "Say hello to the rest of the world..",
  "authors": [
    {
      "name": "Acme Labs"
    }
  ],
  "extra": {
    "tastyigniter-extension": {
      "code": "acme.helloworld",
      "name": "Hello World",
      "icon": {
        "class": "fa fa-puzzle-piece",
        "color": "#FFF",
        "backgroundColor": "#ED561A"
      },
      "require": {
        "igniter.local": "*",
        "igniter.user": "*",
        "igniter.payregister": "*"
      }
    }
  }
}
```

| Field           | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| **name**        | the Composer package's name in vendor/package format, required. You should use a vendor name that is unique to you, such as your GitHub username. You should prefix the package part with `ti-ext-` to indicate that your package is intended for use with TastyIgniter.   |
| **type**        | MUST be set to `tastyigniter-extension`. ensures that your extension will be installed as such when someone "requires" it, required.       |
| **description**  | a one-sentence summary of what the extension does, required. (max. char: 130)  |
| **authors**        | an object to specify the name of the extension author, required.                          |
| **extra.tastyigniter-extension**        | holds TastyIgniter-specific extension metadata, such as your extension's display name and icon style.                          |
| **extra.tastyigniter-extension.code**        | the extension unique identifier code, required.        |
| **extra.tastyigniter-extension.name**        | specifies the extension name, required. The value is used as the extension display name.             |
| **extra.tastyigniter-extension.icon**        | an object that defines the icon for your extension. The **name** property is the name of a [Font Awesome icon class](https://fontawesome.com/icons). All other properties are used as the style attribute for your extension's icon.                         |
| **extra.tastyigniter-extension.homepage**        | specifies the extension website URL, optional.                          |
| **extra.tastyigniter-extension.require**        | defines other TastyIgniter extensions your extension depends on, optional. In the example above, **igniter.cart** extension depends on the **igniter.local** extension.                         |

See [the composer.json schema](https://getcomposer.org/doc/04-schema.md) documentation for information about other
properties you can add to composer.json.

### Readme file

If you want to publish your extension on the [TastyIgniter marketplace](https://tastyigniter.com/marketplace), you must
create a **readme.md** file in a standardized format in your extension directory.

### Routes file

Extensions can also provide a file named **routes.php** with custom routing logic as described in
the [routing article](../advanced/routing).

## Registration

An **Extension.php** file (aka *Extension registration file*) is an essential part of the TastyIgniter extension for providing methods for extending the TastyIgniter core. 

The registration script should define an extension name class which extends the class `\System\Classes\BaseExtension`. The following is an example Extension registration file.

```php
namespace Acme\HelloWord;

class Extension extends \System\Classes\BaseExtension
{
    public function extensionMeta()
    {
        return [
            'name' => 'The extension name',
            'author' => 'The extension author name',
            'description' => 'The extension description',
            'icon' => 'The extension icon. Any FontAwesome icon name, for example: `fa-puzzle-piece`',
        ];
    }
    
    public function register()
    {
        
    }

    public function boot()
    {
        
    }
}
```

> Its recommended to define the extension metadata using the `composer.json` file instead of overriding the `extensionMeta` method.

### Registration class methods

The following methods are supported in the extension registration class:

| Method                         | Description                                                  |
| ------------------------------ | ------------------------------------------------------------ |
| **register()**                 | register method, called when the plugin is first registered. |
| **boot()**                     | boot method, called right before the request route.          |
| **extensionMeta()**            | returns metadata about the extension, when an extension.json is not supplied. |
| **registerComponents()**       | registers any [front-end components](../customize/components) supplied by this extension. |
| **registerNavigation()**       | registers admin navigation menu items for this extension, see below for example. |
| **registerPermissions()**      | registers any [staff permissions](../customize/permissions#registering=permissions) supplied by this extension. |
| **registerSettings()**         | registers any [admin settings page](#registering-settings-link) supplied by this extension. |
| **registerDashboardWidgets()** | registers any [admin dashboard widgets](../extend/widgets#dashboard-widget), supplied by this extension. |
| **registerFormWidgets()**      | registers any [admin form widgets](../extend/widgets#form-widget) supplied by this extension. |
| **registerMailLayouts()**      | registers any mail view layouts supplied by this extension.  |
| **registerMailTemplates()**    | registers any mail view templates supplied by this extension, see below for example. |
| **registerMailPartials()**     | registers any mail view partials supplied by this extension. |
| **registerPaymentGateways()**  | registers any [payment gateways](../advanced/payment-gateways) supplied by this extension. |

An example registering **mail templates view** file `extensions/acme/helloworld/views/mail/contact`:

```php
    public function registerMailTemplates()
    {
        return [
            'acme.helloworld::mail.contact' => 'Contact form email to admin',
        ];
    }

```

An example registering a top-level and child **menu navigation menu item**:

```php
public function registerNavigation()
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
   \Main\Classes\MainController::extend(function($controller) {
        $controller->middleware(\Path\To\CustomMiddleware::class);
    });
    
    // Alternatively, add a new middleware to beginning of the stack.
    $this->app[\Illuminate\Contracts\Http\Kernel::class]
         ->prependMiddleware(\Path\To\CustomMiddleware::class);

    // Alternatively, add a new middleware to end of the stack.
    $this->app[\Illuminate\Contracts\Http\Kernel::class]
         ->pushMiddleware(\Path\To\CustomMiddleware::class);
}
```

An example registering **console commands** supplied by **Igniter.Api** extension:

```php
public function register()
{
    $this->registerConsoleCommand(
        'create.apiresource', \Igniter\Api\Console\CreateApiResource::class
    );
}
```

## Using Third-Party Composer Packages

There are a number of scenarios that require the developer to add a third-party composer packages to their extension.

Here is a full example of how the **Igniter.PayRegister** extension adds third-party package `omnipay/stripe`:

```json
{
  "name": "tastyigniter/ti-ext-payregister",
  "type": "tastyigniter-extension",
  "description": "Allows you to accept credit card payments using PayPal, Stripe, Authorize.Net and/or Mollie.",
  "keywords": [
    "tastyigniter",
    "paypal",
    "stripe",
    "payment",
    "gateway"
  ],
  "license": "MIT",
  "authors": [
    {
      "name": "Sam Poyigi",
      "email": "sam@sampoyigi.com"
    }
  ],
  "require": {
    "php": ">=7.0",
    "omnipay/stripe": "~3.0"
  },
  "extra": {
    "tastyigniter-extension": {
      "code": "igniter.payregister",
      "name": "Pay Register",
      "icon": {
        "class": "fa fa-cash-register",
        "backgroundColor": "#88C425",
        "color": "#1B2707"
      },
      "homepage": "https://tastyigniter.com/marketplace/item/igniter-payregister"
    }
  }
}
```

> Do not commit the `/vendor` directory, the `composer.json` file should be committed to the repository.

### Dependencies install process

The marketplace takes the following extra few steps when preparing extensions that define third-party dependencies using `composer.json`.

- Injects dependencies already included in the TastyIgniter core into the `replace` property of `composer.json`
- Executes `composer install` in the extension directory
- Deletes `composer.lock` files to prevent duplicating dependencies with the core `composer.json`, since a `/vendor`
  folder already exists in the extension directory.
- Lastly, the final result is packaged and ready to be consumed via Update API by the TastyIgniter Update Manager

## Settings and Configuration

There are two ways to configure extensions – with admin settings (database settings) and configuration files.

### Using the settings model

Settings models, like any other models, resides in the **/models** subdirectory of the extension directory.

```yaml
extensions/
  igniter/
    helloworld/
      models/
        config/        			<=== Setting model config directory
          cartsettings.php    	<=== Setting model form fields
        CartSettings.php     	<=== Setting model class file
```

By implementing the `SettingsModel` action class and extending the base `Model` class in a model class, you can create models for storing settings in the database. 

```php
<?php namespace Igniter\Cart\Models;

use Model;

class CartSettings extends Model
{
    public $implement = [System\Actions\SettingsModel::class];

    // A unique code used for saving the settings to the database
    public $settingsCode = 'igniter_cart_settings';

    // Reference to form field model config file, without the .php extension. 
    // Should match the model class name
    public $settingsFieldsConfig = 'cartsettings';
}
```

An example of **writing** to a settings model:

```php
use Igniter\Cart\Models\CartSettings;

// Set a single value
CartSettings::set('cart_key', 'ABCD');

// Set an array of values
CartSettings::set(['cart_key' => 'ABCD']);
```

An example of **reading** from a settings model:

```php
use Igniter\Cart\Models\CartSettings;

// Get a single value
$cartKey = CartSettings::get('cart_key');

// Get a value and return a default value if it doesn't exist
$abandonedCart = CartSettings::get('abandoned_cart', true);
```

### Using the file-based configuration

Extensions can have a `config.php` configuration file in the extension directory `config` subdirectory. Configuration files are PHP scripts that define and return an array, e.g. `/extensions/igniter/cart/config/config.php` configuration file:

```php
<?php

return [
    'destroyOnLogout' => true,
    'cartSessionTtl' => 120
];
```

Use the `Config` class to access the configuration values defined in the configuration file. 

```php
use Config;

// Get a value and return a default value if it doesn't exist
$cartSessionTtl = Config::get('igniter.cart::config.cartSessionTtl', 120);
```

### Registering settings link

An example showing how to create an admin settings link to a settings model. Registered settings links will appear on the **System > Settings** Admin page.

```php
public function registerSettings()
{
    return [
        'cartsettings' => [
            'label' => 'Cart Settings',
            'description' => 'Manage cart settings.',
            'icon' => 'fa fa-cart-plus',
            'model' => Igniter\Cart\Models\CartSettings::class,
            'permissions' => ['Module.Cart'],
        ],
    ];
}
```

## Extending an extension

Why extend an extension with another extension? To keep the changes compatible, if the extension gets an update. Extensions are mainly extended to inject or modify the functionality of the core classes and other extensions using the event service.

The most common place to subscribe to an event is the extension registration file `boot` method.

## Writing tests

If you want to start writing a new extension, remember that unit testing helps you deliver extensions of higher quality.

---
title: "Controllers"
section: extend
sortOrder: 210
---

## Introduction

TastyIgniter Admin implements the MVC pattern. Controllers manage admin pages and implement various features like forms and lists. This article describes how to develop admin controllers and how to configure controller methods.

Controllers are typically stored in the `src/Http/Controllers` directory of an extension. Controller views are `.blade.php` blade views that reside in the `resources/views` directory of an extension. The controller view directory name matches the controller class name written in lowercase. An example of a controller directory structure:

```yaml
author/
  extension/
    src/
      Http/
        Controllers/
          MyController.php          <=== Controller class
    resources/
      views/
        mycontroller/               <=== Controller view directory
          edit.blade.php            <=== Controller view file
```

## Writing controller

### Controller class

Each controller is simply a class that extends the `Igniter\Admin\Classes\AdminController` class. Here is an example of a controller class:

```php
namespace Author\Extension\Http\Controllers;

class MyController extends \Igniter\Admin\Classes\AdminController
{
    public function index() // Controller action
    {
        return $this->makeView('index');
    }
}
```

### Controller properties

Controllers defines a number of properties to configure the controller's behavior. The following properties are available:

| Property               | Description                                                                                                                                                                                          |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `$pageTitle`           | The page title to display in the admin interface.                                                                                                                                                    |
| `$bodyClass`           | The CSS class to apply to the admin page body.                                                                                                                                                       |
| `$currentUser`         | The currently logged-in administrator.                                                                                                                                                               |
| `$defaultView`         | The default view file to render when no view is returned from the action.                                                                                                                            |
| `$suppressView`        | A boolean value that determines whether to suppress the view rendering.                                                                                                                              |
| `$layout`              | The layout file to use for rendering the controller views.                                                                                                                                           |
| `$requiredPermissions` | An array of permission keys that determine the access level needed to access the controller. If an administrator has any of the permissions listed, they are granted access to the controller pages. |
| `$skipRouteRegister`   | A boolean value that determines whether to skip automatically registering the controller routes.                                                                                                     |
| `$guarded`             | An array of controller methods that can not be called as actions. Can be extended in the controller constructor.                                                                                     |

> **Note** These properties can be set in the class definition, in the controller constructor or in the action method.

## Actions, views and routing

Actions are methods within the controller that handle specific tasks. They interact with the model, manipulate data, and load views. Routing is used to map URLs to these controller actions.

For example, the URL for the `index` action of the `MyController` controller in the `Author.Extension` extension would be:

```blade
http://site.com/admin/author/extension/mycontroller/index
```

## Passing data to views

Data can be passed from the controller to the view using the `$this->vars` property. This property is an array that holds the data to be passed to the view. Here is an example of passing data to the view:

```php
public function index()
{
    $this->vars['records'] = Record::all();
}
```

The variables passed with the `$this->vars` property can now be accessed directly in your view:

```php
@foreach ($records as $record)
    {{ $record->name }}
@endforeach
```

## Rendering views

Views are rendered using the `makeView` method. This method takes the view file name as an argument and returns the rendered view. Here is an example of rendering a view:

```php
public function index()
{
    return $this->makeView('index');
}
```

## Setting the navigation context

Extensions can register admin navigation menus and submenus in the [Extension class](../extend/extensions#extension-class-methods). The navigation context can be set in the controller to highlight the active menu item in the admin interface. The `AdminMenu::setContext` method is used to set the navigation context. Here is an example of setting the navigation context:

```php
public function __construct()
{
    parent::__construct();
 
    AdminMenu::setContext('nav-item', 'parent-item');
}
```

## Controller middleware

For example, TastyIgniter includes a middleware that verifies the user of your application is authenticated. If the user is not authenticated, the middleware will redirect the user to the login screen. However, if the user is authenticated, the middleware will allow the request to proceed further into the application.

Controller middleware is executed after the request is processed by TastyIgniter, but before the response is sent to the browser, allowing you to perform actions or modify the request/response during this lifecycle stage.

To define middleware for your controller, you may specify it in the `__construct()` method of your Admin controller by calling the `middleware()` method. Here is an example of defining middleware for a controller:

```php
public function __construct()
{
    parent::__construct();
 
    $this->middleware(function ($request, $response) {
        // Middleware functionality
    });
}
```

You can also control which actions will use your middleware by using the `only()` and `except()` modifiers. For example, to restrict the middleware to only run on the `index` action or on every action except the `index` action, you could do the following:

```php
public function __construct()
{
    parent::__construct();
 
    $this->middleware(function ($request, $response) {
        // Middleware functionality
    })->only('index');
    
    // Or
    
    $this->middleware(function ($request, $response) {
        // Middleware functionality
    })->except('index');
}
```

## Controller action classes

Action classes are behaviours that can be attached to controllers to extend their functionality. Action classes are useful for sharing common controller logic across multiple controllers. To attach an action class to a controller, you can set the `$implement` property on the controller class. 

Here is an example of attaching an action class to a controller:

```php
namespace Author\Extension\Http\Controllers;

class MyController extends \Igniter\Admin\Classes\AdminController
{
    public array $implement = [\Author\Extension\Http\Actions\MyAction::class];
    
    //
}
```

Each controller action is simply a class that extends the `Igniter\System\Classes\ControllerAction` class. Here is an example of a controller action class:

```php
namespace Author\Extension\Http\Actions;

class MyAction extends \Igniter\System\Classes\ControllerAction
{
    public function __construct($controller)
    {
        parent::__construct($controller);
        
        // Configure the action
    }
    
    public function index() // Action method
    {
        return $this->makeView('index');
    }
}
```

## Using AJAX handlers

Controllers can define AJAX handlers to handle AJAX requests. AJAX handlers are methods with the name starting with "on" string that return an array of data, throw an exception or redirect to another page (see [AJAX event handlers](../advanced/ajax-request#using-ajax-handlers)). Here is an example of defining an AJAX handler:

```php
public function onAction()
{
    if (Request::input('someVar') != 'someValue') {
        throw new ApplicationException('Invalid value');
    }
    
    return ['data' => 'value'];
}
```

The AJAX handler can be triggered with the data-request attribute in the view:

```blade
<button
    data-request="onAction"
    data-request-data="someVar: 'someValue'"
    data-request-success="alert('Success!')"
>Click me</button>
```

## Restricting access

Access to controller actions can be restricted using the `$requiredPermissions` property. This property holds an array of permission keys that determine the access level needed. If an administrator has any of the permissions listed, they are granted access to the controller pages. 

Here is an example of how to restrict access to a controller using permissions:

```php
namespace Author\Extension\Http\Controllers;

class MyController extends \Igniter\Admin\Classes\AdminController
{
    public null|string|array $requiredPermissions = ['Author.Extension.ManageRecord'];
    
    //
}
```

See more about [restricting access to admin pages](../extend/permissions#restricting-access-to-admin-pages) and [restricting access to admin controller actions](../extend/permissions#restricting-access-to-admin-controller-actions).
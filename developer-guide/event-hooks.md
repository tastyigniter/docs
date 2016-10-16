---
title: Event Hooks
layout: page
callout: This section is incomplete. Please help to improve it.
---

## System Events

Hooks are provided by TastyIgniter to allow your extension to 'hook into' the rest of TastyIgniter, 
without having to modify the core code; that is, to call Class/functions in your extension at specific times.
For example, ...

Because this is aimed squarely at the developer, and not the end user, there is no GUI to manage this functionality.

TastyIgniter's core provides a number of hooks integrated into the system that you can tap into to modify the behavior or data without modifying the core code itself. 
You tell TastyIgniter that you want to respond to certain events by creating a class/method and editing the `config/events.php` file with the name of the hook and the information where to find the code to run. 
The code can live in any extension's library or model or controller that you choose. This is similar to that provided by the `Bonfire Dev Team`

### Registering An Event

To take an action whenever a new order is created in the system, like creating some new data specific to your extension, you would first create a method in your specific class to handle this. 
For this example, we have chosen to add a `after_create_order()` method in our `Admin_extension` controller, but it could have been in a library or model just as easily.

```php
public function after_create_order($order_id) {
    ...
}
```

We know that the *payload* the event provides is the id of the order, so we capture that in the first parameter. 
At this point, we could use the `Orders_model` to retrieve the order, or save some new settings just for this order, etc.

The next step is to register our event in the configuration file. Open up `config/events.php` in your extension folder and add a new `$config` array.

```php
$config['after_create_order'][] = array(
    'module'        => 'custom_extension',
    'filepath'      => 'controllers',
    'filename'      => 'Admin_extension.php',
    'class'         => 'Admin_extension',
    'method'        => 'after_create_order'
);
```

The following parameters are all required within the config array:

* `module` - The name of the extension to find it in. Must be the same as the folder name of the extension.
* `filepath` - The path to the file within the extension’s folder. This allows you to use any controller, library, or helper file for your event.
* `filename` - The name of the file to load. Must include the file extension.
* `class` - The name of the class to create an instance of.
* `method` - The name of the method within the class to call.

Most system events will deliver a payload. This will typically be an array of data that the event allows you to operate on. 
Each payload will be described in the event descriptions below.

## Events

The following table lists all events within the core of TastyIgniter that are available for you to access.

[*] => not available

### Controllers

Event	 | Description      | Payload   | File(s)
----------|-----------------------|-------------|-----------
before_controller*	 | Called prior to the controller constructor being ran.   | Payload is the name of the current controller. | --    |
after_controller_constructor*	| Called just after the controller constructor is ran, but prior to the method being ran. Payload is the name of the current controller.	| --    |

### Templates and Pages

Event	 | Description      | Payload   | Files(s)
---------|--------------------|-----------------|------
after_page_render*	 | Called just after the main view is rendered.   | The view’s output.	| --    |
after_layout_render*	 | Called just after the entire page is rendered.   | The current output.	| --    |
after_block_render*	 | Called just after the block is rendered.   | An array with ‘block’ being the name of the block and ‘output’ being the rendered block’s output.	| --    |

### Customer

Event	 | Description      | Payload   | File(s)
---------|--------------------|---------------|--------
after_login*	 | Called after successful login.   |An array of `customer_id` and `customer_group_id`.	| --    |
before_logout*	 | Called just before logging the user out.     | An array of `customer_id` and `customer_group_id`.	|-- |

### Orders

Event	 | Description      | Payload   | File(s)
----------|-------------------|---------------|---------
after_create_order	 | Called after an order is created.   | An array of one (order_id) index, where (order_id) is the ID of the just created order.	| Models\Orders_model.php   |
before_order_update*	 | Called just prior to updating an order.   | An array of `order_id` and ‘data’, where data is all of the update information passed into the method. Note: `order_id` may be an array if the `Orders_model`'s `updateOrder()` method is called with an array as the first parameter. In this case, the order's ID may not be in the array...	| Models/Orders_model.php   |
after_order_update*	 | Called just after updating an order.   | An array of `order_id` and ‘data’, where data is all of the update information passed into the method. Note: `order_id` may be an array if the `Orders_model`'s `updateOrder()` method is called with an array as the first parameter. In this case, the order's ID may not be in the array...	| Models/Orders_model.php   |

### Cart Module
Event	 | Description      | Payload   | File(s)
----------|-------------------|---------------|---------
cart_module_before_cart_totals	 | 	Called before taxes is calculated and totals is displayed | --	 | extensions/cart_module/controllers/Cart_module.php

## Using Events In Your Extensions

You can use events in your own extensions to provide places for other extensions to hook into. It is very simple and only requires that you use a single line of code in your library, model, or controller wherever you would like the event to fire.

```php
Events::trigger('event_name', $payload);
```

The function takes two parameters.

The first parameter is the name of the event to call. You may use any name that you want, so long as it doesn't conflict with an existing event name. 
To avoid collisions, it is recommended that you prefix your event name with the name of your extension, like `my_extension.after_comment`.

The second parameter is a single variable that contains the payload that you wish to provide to the event responders. This could be an ID, an array with multiple pieces of data, or anything else that is appropriate to your needs.
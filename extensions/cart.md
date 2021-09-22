---
title: "Cart Extension"
section: "extensions"
sortOrder: 40
---

## Introduction

TastyIgniter Cart extension is simple, flexible shopping cart. 

## Features
- Stock control
- Cart conditions: Tip, Tax
- Order types: Delivery, Pick-up
- Abandoned Checkout
- Payment method: Cod, Paypal, Stripe
- One page checkout
- Easy to extends and customize
- Sending order confirmation emails

## Installation

To install this extension, click on the **Add to Site** button on the TastyIgniter marketplace item page or search for **Igniter.Cart** in **Admin System > Updates > Browse Extensions**

## Admin Panel
In the admin user interface you can manage the cart conditions.

## Components
| Name     | Page variable                  | Description                                      |
| -------- | ------------------------------ | ------------------------------------------------ |
| CartBox  | `@component('cartBox')`  | Show the contents of and manages the user's cart |
| Checkout | `@component('checkout')` | Displays Checkout form on the page               |
| OrderPage | `@component('orderPage')` | Displays a single order on the page               |
| Orders | `@component('orders')` | Displays a list of orders on the page               |

### CartBox Component

**Properties**

| Property                 | Description              | Example Value | Default Value |
| ------------------------ | ------------------------ | ------------- | ------------- |
| showCartItemThumb     | Show cart menu item image in the popup  | true/false        | false         |
| cartItemThumbWidth     | Cart item image width   | 720        | 720         |
| cartItemThumbHeight     | Cart item image height    | 300        | 300         |
| checkStockCheckout     | Check cart item stock quantity            | true/false         | false         |
| pageIsCheckout      | Value to determine if the user is on a checkout page             | true/false         | false         |
| pageIsCart          | Display a standalone cart             | true/false         | false         |
| hideZeroOptionPrices          | Whether to hide zero prices on options            | true/false         | false         |
| checkoutPage          | Checkout page path            | checkout/checkout         | checkout/checkout         |
| localBoxAlias          | Specify the LocalBox component alias used to refresh the localbox after the order type is changed            | localBox         | localBox         |

**Variables available in templates**

| Variable                  | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `{{ $cart }}` | Cart Class instance                                                |

**Example:**

```
---
title: 'Checkout'
permalink: /checkout

'[cartBox]':
    cartBoxTimeFormat: 'ddd hh:mm a'
    checkStockCheckout: 0
    pageIsCheckout: 0
    pageIsCart: 0
    checkoutPage: checkout/checkout
---
...
@component('cartBox')
...
```

### Checkout Component

**Properties**

| Property                 | Description              | Example Value | Default Value |
| ------------------------ | ------------------------ | ------------- | ------------- |
| showCountryField                     | Show/hide the country checkout field            | true/false        | false         |
| showAddress2Field                     | Show/hide the address 2 checkout field            | true/false        | true         |
| showCityField                     | Show/hide the city checkout field            | true/false        | true         |
| showStateField                     | Show/hide the state checkout field            | true/false        | true         |
| agreeTermsPage                     | Terms and conditions page            |    page/terms     |          |
| menusPage                     | Menus page            |    page/menus     |          |
| redirectPage                     | Redirect page name           |    checkout/checkout    |    checkout/checkout      |
| orderPage                     | Order page name            |    account/order     |     account/order     |
| successPage                     | Order confirmation page name           |    checkout/success     |     checkout/success     |
| successParamCode                     | URL routing code used for displaying the order confirmation page            | hash       | hash         |

**Variables available in templates**

| Variable                  | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `{{ $order }}` | Order Model instance                                          |
| `{{ $paymentGateways }}` | Instances of available payment gateways                                          |

**Example:**

```
---
title: 'Checkout'
permalink: /checkout

'[checkout]':
    showCountryField: 0
    showAddress2Field: 1
    showCityField: 1
    showStateField: 1
    menusPage: local/menus
    redirectPage: checkout/checkout
    successPage: checkout/success
    successParamCode: 'hash'
---
...
@component('checkout')
...
```

## Registering a new Cart Condition

Here is an example of an extension registering a cart condition.

```
public function registerCartConditions()
{
    return [
        \Igniter\Local\CartConditions\Tip::class => [
            'name' => 'tip',
            'label' => 'Tip',
            'description' => 'Applies tips to cart total',
        ]
    ];
}
```

## Automations

## Events
- Order Placed Event
- Order Status Update Event
- Order Assigned Event

## Conditions
- Order Attributes
- Order Status Attributes

## Notifications

- Order confirmation notification
- Order status update notification
- Order assigned notification

## Events

The Cart Library used with this extension will fire some global events that can be useful for interacting with other extensions.

| Event | Description | Parameters |
| ----- | ----------- | ---------- |
| `cart.created` |    When cart instance is created.    |           |
| `cart.added` |      When an item has been added to the cart.       |      The `CartItem` instance     |
| `cart.updated` |     When an item has been updated in the cart.     |      The `CartItem` instance     |
| `cart.removed` |    When an item has been removed from the cart.      |     The `CartItem` instance      |
| `cart.cleared` |     When all items has been cleared from the cart.     |           |
| `cart.condition.loaded` |      When a condition has been loaded to the cart.    |      The `CartCondition` instance     |
| `cart.condition.removed` |     When a condition has been removed from the cart.     |     The `CartCondition` instance      |
| `cart.condition.cleared` |      When all condition has been cleared from the cart.    |           |
| `cart.stored` |    When the content of a cart was stored.      |           |
| `cart.restored` |      When the content of a cart was restored.    |           |

**Example of hooking an event**

```
Event::listen('cart.updated', function($cartItem) {
    // ...
});
```
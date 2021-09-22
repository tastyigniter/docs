---
title: "User Extension"
section: "extensions"
sortOrder: 110
---

## Introduction

Management of front-end users (customers) on TastyIgniter.

## Installation

To install this extension, click on the **Add to Site** button on the TastyIgniter marketplace item page or search for **Igniter.User** in **Admin System > Updates > Browse Extensions**

## Admin Panel
In the admin user interface you can manage customers and their groups.

## Components
| Name     | Page variable                | Description                                      |
| -------- | ---------------------------- | ------------------------------------------------ |
| Session | `@component('session')` | Manages the logged or guest user's session              |
| Account | `@component('account')` | Provides the sign in form, registration form and update form |
| ResetPassword | `@component('resetPassword')` | Allows a user to reset their password if they have forgotten it.               |
| AddressBook | `@component('addressBook')` | Manages the users delivery addresses               |

### Session Component

**Properties**

| Property                 | Description              | Example Value | Default Value |
| ------------------------ | ------------------------ | ------------- | ------------- |
| security                     | Restrict a page or layout to only signed in users, only guests or no restriction            | all/guest/customer        | all        |
| redirectPage                     | Page name to redirect to when access is restricted           | home         |   home |

**Variables available in templates**

| Variable                  | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `{{ $customer }}` | Instance of the logged user model                                                |

**Example:**

```
@if ($customer)
    <p>Hello {{ $customer->first_name }}</p>
@else
    <p>Not logged in</p>
@endif

@auth
    // The customer is logged in...
@endauth

@guest
    // The customer is not logged in...
@endguest
```
**Logout Example:**
```
<a data-request="session::onLogout" data-request-data="redirect: '/home'">Logout</a>
```

### Account Component

**Properties**

| Property                 | Description              | Example Value | Default Value |
| ------------------------ | ------------------------ | ------------- | ------------- |
| accountPage                     | Account page name                  | account/account         |   account/account        |
| addressPage                     | Address book page name                  | account/address         |   account/address       |
| ordersPage                     | Orders page name                  | account/orders         |   account/orders        |
| reservationsPage                     | Reservations page name                  | account/reservations         |   account/reservations        |
| reviewsPage                     | Reviews page name                  | account/reviews         |   account/reviews        |
| inboxPage                     | Inbox page name                  | account/inbox         |   account/inbox        |
| loginPage                     | Login page name                  | account/login         |   account/login        |
| agreeRegistrationTermsPage     | Terms page name                  |          |        |
| redirectPage                     | Page name to redirect to when login or registration is successful            | account/account      |   account/account        |

**Variables available in templates**

| Variable                  | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `{{ $customer }}` | Instance of the logged user model                                                |

**Example:**

```
---
title: 'Account Details'
permalink: /account

'[account]': { }
---
...
@component('account')
...
```

**Login Form Example**
```
---
title: 'Account Login'
permalink: /login

'[account]': { }
---
...
@partial('account::login')
...
```

**Register Form Example**
```
---
title: 'Account Register'
permalink: /register

'[account]':
    agreeRegistrationTermsPage: 12
---
...
@partial('account::register')
...
```

### Reset Password Component

**Properties**

| Property                 | Description              | Example Value | Default Value |
| ------------------------ | ------------------------ | ------------- | ------------- |
| resetPage                     | Page name to redirect to after reset link has been sent                | account/reset         |   account/reset  |
| loginPage                     | Account login page name                  | account/login         |   account/login    |
| paramName                     | Route parameter name to find the reset code                  | code         |   code  |

**Example:**

```
---
title: 'Reset Password'
permalink: /forgot-password/:code?

'[resetPassword]': { }
---
...
@component('resetPassword')
...
```

## Events

This extension will fire some global events that can be useful for interacting with other extensions.

| Event | Description | Parameters |
| ----- | ----------- | ---------- |
| `igniter.user.beforeAuthenticate` | Before the user is attempting to authenticate    |      [ $component, $credentials ]    |
| `igniter.user.beforeRegister` | Before the user is attempting to register  |        [ &$postData ]  |
| `igniter.user.login` | The user has logged in successfully  |         [ $component ]    |
| `igniter.user.logout` | The user has logged out sucessfully  |        [ $customer ] |
| `igniter.user.register` | The user has registered successfully |        [ $customer, $postData ]    |

**Example of hooking an event**

```
Event::listen('igniter.user.logout', function($customer) {
    // ...
});
```
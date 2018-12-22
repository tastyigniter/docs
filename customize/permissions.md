---
layout: page
section: extending
title: "Permissions"
callout: This section is incomplete. Please help to improve it.
---

## Introduction

## How Permissions Work

## Registering permissions

The next example shows how to register admin staff permission items. Permissions are defined with a permission key and description. Admin controllers can use extension permissions to restrict user access to pages or features.

```php
public function registerPermissions()
{
    return [
        'Igniter.HelloWorld' => [
            'action' => ['access', 'add', 'manage', 'delete'],
            'description' => 'The permission description',
        ],
    ];
}
```

## Restricting admin pages

### 
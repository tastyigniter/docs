---
title: "Permissions"
section: customize
sortOrder: 290
---

## Introduction

The Permissions system in TastyIgniter is designed to manage access and restrictions to the admin interface and its features. Permissions are assigned to staff roles, and staff roles are assigned to staff members. Staff members inherit the permissions of the roles they are assigned.

Super Admins are identified by the `super_user` flag set to `true`. Below them are Administrators who have varying levels of permissions. Super Admins have unrestricted access to the entire system and can only be managed by themselves or other Super Admins. Regular administrators, regardless of whether they possess the `Admin.Staff` permission, cannot access or modify Super Admin accounts.

## How Permissions Work

Permissions in TastyIgniter are represented by string keys formatted as `Author.Extension.Permission`. These permissions are assigned to an administrator via the staff's role, which inherits specific permissions.

For instance, if administrator _Sam_ is part of the _Chef_ role and the _Chef_ role includes the `Admin.AssignOrder` permission, Sam will inherit the ability to manage orders. Staff Roles function as named groups of permissions, defining the role of an administrator within the system. A staff member can be assigned only one role at a time, but multiple staff members can share the same role. TastyIgniter comes with four predefined system roles: `owner`, `manager`, `waiter`, and `delivery`. Additionally, you can create and assign any number of custom roles with unique permission combinations to administrators.

It's important to note that Staff Groups are unrelated to permissions. They serve purely as an administrative tool for organizing administrators. For example, groups can be used to assign tasks to all administrators in the Kitchen, facilitating management and coordination.

## Registering permissions

Administrator permissions in TastyIgniter can be registered by extensions by overriding the `registerPermissions` method in the [Extension class](../extend/extensions#extension-class). Permissions are defined as an array where the keys are the permission name, and the values provide details about the permission group and descriptions. Each permission key follows a format that includes the name of the author, the name of the extension, and the name of the function.

Here is an example of how permissions might be registered:

```php
public function registerPermissions(): array
{
    return [
        'Author.Extension.ManageSettings' => [
            'group' => 'Configuration',
            'label' => 'Manage extension settings'
        ],
    ];
}
```

In this example, each key `Author.Extension.ManageSettings` represents a specific action or capability within the extension that can be enabled or disabled per administrator. The `group` and `label` provide context and description for these permissions, making it easier to understand and manage within the admin interface.

## Wildcard permissions

Additionally, you can use the asterisk symbol (`*`) to represent a wildcard, indicating the "all permissions" condition within a specific scope. This allows any administrator with permissions starting with a specific prefix to access the controller pages. For example:

```php
public $requiredPermissions = ['Author.Extension.*'];
```

This setup means that any administrator with permissions that begin with `Author.Extension.` (such as `Author.Extension.Create`, `Author.Extension.Edit`, etc.) will have access to the pages controlled by the `StaticPages` controller. This wildcard approach is useful for granting access to multiple permissions at once.

## Restricting access to admin pages

In the [admin controller](../extend/controllers) class of TastyIgniter, you can specify the required permissions for accessing the controller's actions using the `$requiredPermissions` property. This property holds an array of permission keys that determine the access level needed. If an administrator has any of the permissions listed, they are granted access to the controller pages.

Here is an example of how to restrict access to a controller using permissions:

```php
<?php namespace Author\Extension\Http\Controllers;

class MyController extends \Igniter\Admin\Classes\AdminController
{
    public null|string|array $requiredPermissions = ['Author.Extension.ManageRecord'];
}
```

In this example, only administrators with the `Author.Extension.ManageRecord` permission can access the `StaticPages` controller.

## Restricting access to admin controller actions

Additionally, you have the flexibility to apply permissions to specific actions within a controller by using method names as keys in the `$requiredPermissions` array. This granular control allows you to define precisely which administrative roles can access certain functionalities.

For instance, if you want to restrict access to deleting records to only those administrators who have the appropriate permissions:

```php
<?php namespace Author\Extension\Controllers;

class MyController extends \Igniter\Admin\Classes\AdminController
{
    public null|string|array $requiredPermissions = [
        'Author.Extension.ManageRecord'
        'delete' => 'Author.Extension.DeleteRecord'
    ];
}
```

## Restricting access to features

In TastyIgniter, the `\Igniter\User\Models\User` user model includes a method named `hasPermission` for determining whether the administrator has specific permissions. This functionality is particularly useful for implementing access controls within the admin user interface, ensuring that only authorized users can access certain features or data.

The `hasPermission` method can be called with either a single permission key string or an array of key strings. Additionally, there is an optional parameter that allows you to specify whether all listed permissions are required (`true`) or if having any one of the permissions is sufficient (`false`). By default, this parameter is set to `false`, meaning the function will return `true` if any of the specified permissions match.

This method will always return `true` for administrators with the `super_user` flag set to `true`, indicating they are superadmins and have unrestricted access. For other users, it checks the permissions assigned through their role or group.

Here an example using the `hasPermission` method in the controller code:

```php
// Check if the user has any permission within the 'Author.Extension' scope
if ($this.user.hasPermission('Author.Extension.*')) {
    //
}

// Check if the user has both 'ManagePages' and 'ManageMenus' permissions
if ($this.user.hasPermission([
    'Author.Extension.ManagePages',
    'Author.Extension.ManageMenus'
], true)) {
    // Execute code only if the user has both permissions
}
```

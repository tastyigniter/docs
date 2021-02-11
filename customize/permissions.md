---
title: "Permissions"
section: customize
sortOrder: 160
---

## Introduction

The Permissions system controls access and restriction to all parts of a TastyIgniter application. There are Super Admins at the lowest level (users with the `super_user` flag set to `true`), and Administrators (staff), and permissions. The `\Admin\Models\Staffs_model` and `\Admin\Models\Users_model` models are the containers that hold all the important information about an administrator.

Superadmins have access to everything in the system and can only be managed by themselves or by other superadmins; they are not accessible to or editable by regular administrators, even if an administrator has the `Admin.Staff` permission.

## How Permissions Work

Permissions are string keys in the form of `Author.Extension.Permission` name which are given to an administrator by inheritance through the staff's Role.

When determining whether a staff has a specific permission, the permission settings for that staff's role will be applied. For example, if administrator **Sam** belongs to **Chef** role, and role **Chef** has `manage_order` permission, then **Sam** will get to `manage_order`.

Staff Roles are groups of permissions with a name used to identify the role of an administrator. Only one role can be assigned to a staff at once. Multiple staff may be assigned a role. TastyIgniter ships with four system roles by default; `owner`, `manager`, `waiter` and `delivery`. Any number of custom roles can be created and applied to administrators with their own combinations of permissions.

Staff Groups have nothing to do with permissions and are strictly an administrative tool for grouping administrators. For instances, groups can be used to assign an order to all administrators in the Kitchen.

## Registering permissions

Administrator permissions can be registered by extensions by overriding the `registerPermissions` method within the [Extension registration class](../extend/extensions#registration). The permissions are defined as an array with keys that match the permission keys and the values holding the permission group & descriptions. The permission keys consist of the name of the author, the name of the extension and the name of the function. Here is an example:

```yaml
Igniter.Cart.Manage
```

Admin controllers can use permissions defined by extensions to restrict administrator access to pages or features. Permissions are defined with a permission key and description. The next example shows how to register administrator permission items.

```php
public function registerPermissions()
{
    return [
        'Igniter.Pages.Manage' => [
            'description' => 'The permission description',
            'group' => 'module',
        ],
    ];
}
```

## Restricting access to admin pages

Within an admin controller class, you may specify which permissions are required to access the controller pages, using the `$requiredPermissions` controller's property. This property should contain an array of permission keys. The system would allow the administrator to access the controller pages if the administrator permissions match ANY permission from the list.

```php
<?php namespace Igniter\Pages\Controllers;

use Admin\Classes\AdminController;

class StaticPages extends AdminController
{
    public $requiredPermissions = ['Igniter.Pages.Manage'];
}
```

The asterisk symbol can also be used to signify the "all permissions" condition. The controller pages are available in the next example for all administrators who have any permissions beginning with the "Igniter.Pages." string:

```php
public $requiredPermissions = ['Igniter.Pages.*'];
```

## Restricting access to features

The admin `\Admin\Models\Users_model` user model has method `hasPermission` for determining whether the administrator has specific permissions. To restrict the functionality of the admin user interface, you can use this feature. This method takes two parameters: the permission key string (or an array of key strings) and the optional parameter specifying that all permissions specified with the first parameters are required. 

The `hasPermission` method returns `true` for any permission if the administrator is a superadmin (`super_user` set to `true`) or if the administrator does have the specified permissions through their group. The example below illustrates how to use the methods in the controller code:

```php
if ($this->user->hasPermission('Igniter.Pages.*')) {
    // ...
}

if ($this->user->hasPermission([
    'Igniter.Pages.ManagePages',
    'Igniter.Pages.ManageMenus'
])) {
    // ...
}
```
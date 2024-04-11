---
title: "Upgrade from v3.x"
section: "getting-started"
sortOrder: 30
---

The following is an instructional guide on how to upgrade your v3.x TastyIgniter installation to the latest version v4.x

TastyIgniter has been rewritten as an installable Laravel package. This is a huge change that affects almost
every part of the codebase.

## Backing Up The Database

Use this command to backup your MySQL database. For example, if the username is `root` and the database is called
`database_name`. You will then be prompted to enter the password.

```bash
mysqldump -u root -p database_name > tastyigniter_backup.sql
```

To restore the backup, you can use this command.

```bash
mysql -u root -p database_name < tastyigniter_backup.sql
```

## New requirements

- **PHP 8.2+** with the following extensions: bcmath, pdo_mysql, ctype, curl, openssl, dom, gd, exif, mbstring, json,
  tokenizer, zip, xml
- **MySQL 5.7+** or **MariaDB 10.3+** or **PostgreSQL 10.0**

## Upgrading from v3.x

> Before you get started, it's a good idea to back up your website. This means if there are any issues you can restore
> your website.

Install TastyIgniter 4.0 in a new directory

```bash
composer create-project tastyigniter/tastyigniter mytasty-new
```

To make the configuration process run smoothly, you can copy your old configuration to the new website. This step is
optional

```bash
cp mytasty/.env mytasty-new/
```

To avoid broken media, you can either copy or move the `assets/media` directory to the storage directory on the new
installation.

```bash
cp assets/media storage/
```

If you have custom extensions and themes you have developed yourself, you can copy them to the new installation after
upgrade is complete.

Proceed through the installation and database migration. Change directory to the new installation and running the
installation command.

```bash
cd mytasty-new
php artisan igniter:install
```

Finally, complete the upgrade, replace the old installation directory with the new installation.

```bash
cd ..
mv mytasty mytasty-old
mv mytasty-new mytasty
```

### High impact changes

#### TastyIgniter As A Package

TastyIgniter can now be included in an existing Laravel application as a package.
See [Package Installation](/installation#package-installation) for more details.

#### Code Structure

The codebase has been refactored and restructured to follow Laravel conventions which enhance maintainability and
extensibility. This includes updated namespaces, relocated controllers, improved models, and form requests.

- Classes under `Admin\\`, `Main\\`, `System\\` have moved to `Igniter\\Admin\\`, `Igniter\\Main\\`, `Igniter\\System\\`
  respectively.

#### Admin Login with Email

The admin login process now uses email addresses instead of usernames, providing a more convenient and familiar login
experience for administrators.

#### Mailable Integration

Sending registered mail templates now uses Laravel's Mailable classes instead of custom logic. This provides a more
standardized and maintainable approach to sending emails.

#### Mail Template Namespaces

Mail template namespaces have been renamed for consistency. `admin::` is
now `igniter.admin::`, `main::` is now `igntier.main::`, and `system::` is now `igniter.system::`.

#### Translation String Keys

Translation string keys have been updated to follow a consistent naming
convention. `admin::lang.` is now `igniter::admin.`, `main::lang.` is now `igntier::main.`, and `system::lang.` is
now `igniter::system.`.

#### The Singleton Trait

Singletons have been dropped, you can resolve Manager classes through service containter. This allows for better code
organization and easier unit testing. `ExtensionManager::instance()` is now `resolve(ExtensionManager::class)`.

#### Blade Directives

New Blade directives have been introduced to simplify theme development. The `@themeContent` directive
allows rendering of content template files, while `@themePage` replaces `@page` used for rendering page contents.
Additionally,
`@componentPartial` and `@themePartial` directives replace the previous `@component` and `@partial` directives.

### Medium impact changes

#### Extension Configuration

Extension configuration files are no longer merged automatically. Developers must use Laravel's
mergeConfigFrom() method to merge configuration files from extension class `register` method.

#### Database Changes

Several database changes have been made, including merging `staffs` and `users` records into the `admin_users` table,
renaming `staff_groups` to `user_groups`, prefixing user-related tables with `admin_`, dropping conflicting tables, and
increasing the length of varchar to 255 on existing columns.

### Low impact changes

#### Admin Controller Actions

New base view files have been introduced for common admin controller actions such as index, edit, create, and preview.
This eliminates the need to create these view files for your custom controller action.

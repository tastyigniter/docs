---
title: "Installation"
section: "getting-started"
sortOrder: 10
---

Before you proceed with installing TastyIgniter, you should check that your server meets the minimum system
requirements.

## Minimum Requirements

These are the requirements for your web hosting to run TastyIgniter:

- **Apache** (with mod_rewrite enabled) or **Nginx**
- **MySQL 5.7+** or **MariaDB 10.0.5+**
- **PHP 7.3+** with the following extensions: pdo_mysql, curl, openssl, dom, gd, mbstring, json, pdo_mysql, tokenizer,
  zip

## Install TastyIgniter

There are two ways you can install TastyIgniter, either using the Quick or Command-line installation instructions.

### Quick Installation

1. [Download](https://tastyigniter.com/download) and unzip the TastyIgniter setup wizard into an empty directory on your
   server.
2. Create a MySQL user database for TastyIgniter on your database server.
3. Upload the TastyIgniter folders and files to your server. Normally the setup.php file will be at the web root
   directory.
4. Grant write permissions on the setup directory, its subdirectories and files.
4. Run the TastyIgniter setup script by accessing setup.php in your web browser. Example, http://example.com/setup.php
   or http://example.com/folder/setup.php
5. Follow all onscreen instructions and make sure all installation requirements are checked.

<img src="https://tasty-cms.wip/assets/ui/images/mockups/SetupWizard.png" alt="TastyIgniter setup wizard screenshot" style="max-height:250px" />

### Command-line installation

TastyIgniter also uses <a href="https://getcomposer.org/" target="_blank">composer</a> to manage its dependencies,
you'll need to have composer installed on your machine before using the TastyIgniter command-line installation. Run this
command in an empty location that you want TastyIgniter to be installed in:

```bash
composer create-project tastyigniter/tastyigniter .
```

After running the above command, run the install command and follow the instructions to complete installation

```bash
php artisan igniter:install
```

The install command will guide you through the process of setting up TastyIgniter for the first time. It will ask for
the database configuration, application URL and administrator details.

### Command-line unattended installation

Some installations require an unattended mode so that the application can easily be built into automated infrastructure
pipelines and build tools, e.g. Docker.

To run this, its similar to the above command, just instead we provide all of the option values within your projects `.env` and pass the `--no-interaction` flag:

```bash
php artisan igniter:install --no-interaction
```

## Post-installation steps

For security reasons, if you used the [Quick Installation Setup Wizard](#quick-installation), you should delete the
setup files. TastyIgniter will never automatically delete files from your system, so these files and directories should
be deleted manually:

```yaml
setup/      	<== Setup directory
  setup.php     <== Setup script
```

### Setting up the task scheduler

You should add the following Cron entry to your server for scheduled tasks to function properly. Crontab editing is
usually done with the command `crontab -e`.

```bash
* * * * * php /path/to/artisan schedule:run >> /dev/null 2>&1
```

Be sure to replace `/path/to/artisan` with the absolute path to the artisan file in your TastyIgniter root directory .
This Cron will call the command scheduler every minute. When executing the `schedule:run` command, TastyIgniter will
assess your scheduled tasks and run the tasks that are due.

> Task Scheduling is how scheduling time-based tasks are managed in TastyIgniter. Several core features of TastyIgniter, such as checking for updates, use the scheduler.

### Setting up the queue daemon

By default, the queue in TastyIgniter is synchronous and will attempt to run tasks such as sending emails in real time.
This behaviour can be set to an asynchronous method by changing the `default` parameter in the `config/queue.php`.

If you are using the `database` queue, it is a good idea to run the queue process as a daemon service. Use the following
command:

```bash
php /path/to/artisan queue:work
```

You can use Supervisor process monitor to automatically restart the `queue:work`
command if it fails.

For more information on configuring Supervisor and using Queues, consult
the <a href="https://laravel.com/docs/queues#running-the-queue-listener" target="_blank">Laravel Queue docs</a>.

## Web server configuration

TastyIgniter has basic configuration that should be applied to your webserver. Common webservers and their configuration
can be found below.

### Apache configuration

There are some extra system requirements if your webserver is running Apache, `mod_rewrite` should be installed and
enabled and the `AllowOverride` option should be switched on.

TastyIgniter includes a `.htaccess` file – make sure it’s been uploaded correctly.

You will need to uncomment this line in the `.htaccess` file in some cases:

```html
## !IMPORTANT! You may need to uncomment the following line for some hosting environments,
## If your installation resides in a subdirectory, enter the name here also
##
# RewriteBase /
```

If you've created a subdirectory, you can add the subdirectory name as well:

```html
RewriteBase /mysubdirectory/
```

### Nginx configuration

Make sure that `.nginx.conf` file included with TastyIgniter has been uploaded correctly. Then, assuming you have Nginx
setup, add the following to your server’s configuration block:

```html
include /path/to/tastyigniter/.nginx.conf;
```

## Application configuration

### Debug mode

The debug setting is found in the `config/app.php` configuration file with the `debug` parameter, and is disabled by
default.

When enabled, this setting will display detailed error messages when they occur along with other debugging functions.
Debug mode should always be disabled in a live production site. This prevents the display of potentially sensitive
information to the end user.

> Important: Always set the `APP_DEBUG` setting to false in production environments.

### CSRF protection

TastyIgniter offers a simple method to protect your application from cross-site request forgeries.

For every active user session managed by the application, TastyIgniter automatically generates a CSRF "token." This
token is used to check that the authenticated user is the one who actually makes the client requests.

Although CSRF security is enabled by default, you can disable it in the `config/system.php` configuration file using
the `enableCsrfProtection` parameter.

### Bleeding edge updates

TastyIgniter core and some marketplace extensions will introduce changes in two stages to ensure overall stability and
integrity of the codebase. This means that besides the regular stable versions, they do have a test version.

By modifying the `edgeUpdates` parameter in the `config/system.php` configuration file you can instruct the application
to choose test versions from the marketplace.

```php
/*
|--------------------------------------------------------------------------
| Bleeding edge updates
|--------------------------------------------------------------------------
|
| If you are developing with TastyIgniter, it is important to have the latest
| code base, set this value to 'true' to tell the platform to download
| and use the development copies of core files and extensions.
|
*/

'edgeUpdates' => true,
```

When you are using Composer to handle updates, then replace the default TastyIgniter requirements in
your `composer.json` file with the following to receive updates from the develop branch directly.

```json
"tastyigniter/flame": "dev-develop as 1.0",
"laravel/framework": "6.0.*@dev",
```

## Getting Started

You can access the administrator panel from `/admin` with your username and password asked during the setup process.
After you've logged in you'll be able to access the administration panel to configure your site.

> Follow the getting started steps on the administration panel dashboard.

## Getting help

- If you believe you have found a bug, please report it using
  the <a href="https://github.com/tastyigniter/TastyIgniter/issues" target="_blank">GitHub issue tracker</a>, or better
  yet, fork the repo and submit a pull request.
- If you have feedback to share, ideas you would like implemented, by all means, share them on
  the <a href="https://forum.tastyigniter.com" target="_blank">TastyIgniter Community Forums</a>.
- <a href="https://tastyigniter.com/discord" target="_blank">Join us on Discord</a> to chat with us.

## Troubleshooting

1. **A 404 error page is displayed:** This could be a result of the mod_rewrite module not being activated/installed or
   configured properly. Activate mod_rewrite for the Apache web-server. If it is already activated, check the root
   htaccess file in `/.htaccess`, to make sure the `RewriteBase` value is configured properly.
2. **A blank screen is displayed when opening the application:** Check the file permissions are set correctly on
   the `/storage` files and folders and writable for the web server. Also check that your files are owned by the correct
   group and user.
3. **Setup successful but storefront links are not working:** Check that the theme's required extensions are all
   installed.

> **Note:** A detailed installation log can be found in the `setup/setup.log` file.
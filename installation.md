---
title: "Installation"
section: "getting-started"
sortOrder: 10
---

As of Version 4, TastyIgniter can be installed as a [stand-alone](#stand-alone-installation) application, or as a [package](#package-installation) inside an existing Laravel application.

## Stand-alone Installation

### Requirements

These are the requirements to run TastyIgniter as a stand-alone application:

- **Apache** (with mod_rewrite enabled) or **Nginx**
- **PHP 8.2+** with the following extensions: bcmath, pdo_mysql, ctype, curl, openssl, dom, gd, exif, mbstring, json,
  tokenizer, zip, xml
- **MySQL 5.7+** or **MariaDB 10.3+** or **PostgreSQL 10.0**
- **Composer 2.0** or **higher** (for installing dependencies)

### Installing TastyIgniter

TastyIgniter manages its dependencies and extensions using <a href="https://getcomposer.org/" target="_blank">composer</a>.
To install the platform, use the `create-project` command in the terminal to create a project. The command
below creates a new project in the directory `mytasty`.

```bash
composer create-project tastyigniter/tastyigniter:^v4.0@beta mytasty
```

From here, you can move on to the [Setting up TastyIgniter](#setting-up-tastyigniter) step.

## Package Installation

### Requirements

These are the requirements to run TastyIgniter as a package in a Laravel application:

- **Laravel 10+**
- **MySQL 5.7+** or **MariaDB 10.3+** or **PostgreSQL 10.0**
- **Composer 2.0** or **higher** (for installing dependencies)

### Installing TastyIgniter

To install TastyIgniter as a package from your command line, run the following command in your Laravel project
directory:

```bash
composer require tastyigniter/core:^v4.0@beta
```

From here, you can move on to the [Setting up TastyIgniter](#setting-up-tastyigniter) step.

## Setting up TastyIgniter

TastyIgniter includes a command-line setup tool that will get you up and running in a few minutes. It will attempt to
set up TastyIgniter and create an admin user account.

In the TastyIgniter installation's root directory, run the following command:

```bash
php artisan igniter:install
```

The setup command will guide you through the process of setting up TastyIgniter for the first time. It will ask
for the database configuration, application URL and administrator details.

> You can safely run the command multiple times if needed.

**Command-line unattended setup**

Some setup require an unattended mode so that the application can easily be built into automated infrastructure
pipelines and build tools, e.g. Docker.

To run this, it is similar to the above command, just instead we provide all the option values up-front within the
projects
`.env` and pass the `--no-interaction` flag to the installation script:

```bash
php artisan igniter:install --no-interaction
```

## Post-installation steps

There are some things you may need to set up after the installation is complete.

### Directory permissions

Once TastyIgniter is installed, you must grant the non-root user the necessary permissions so that TastyIgniter and Laravel can write to the required system directories.

```bash
sudo chmod -R 755 /path/to/tastyigniter
sudo chown -R www-data:www-data /path/to/tastyigniter
```

> You should never set any folder or file to permission level **777**, as this permission level allows anyone to access the content of the folder and file regardless of user or group.

### Setting up the task scheduler

You should add the following Cron entry to your server for scheduled tasks to function properly. Crontab editing is
usually done with the command `crontab -e`.

```bash
* * * * * php /path/to/artisan schedule:run >> /dev/null 2>&1
```

Be sure to replace `/path/to/artisan` with the absolute path to the artisan file in your TastyIgniter root directory .
This Cron will call the command scheduler every minute. When executing the `schedule:run` command, TastyIgniter will
assess your scheduled tasks and run the tasks that are due.

> Task Scheduling is how scheduling time-based tasks are managed in TastyIgniter. Several core features of TastyIgniter,
> such as assigning orders and checking for updates, use the scheduler.

### Setting up the queue daemon

By default, the queue in TastyIgniter is synchronous and will attempt to run tasks such as sending emails in real time.
This behaviour can be set to an asynchronous method by updating the `QUEUE_CONNECTION` variable in your application's
`.env` file.

It is a good idea to run the queue process as a daemon service. Use the following
command:

```bash
php /path/to/artisan queue:work
```

You can use Supervisor process monitor to automatically restart the `queue:work`
command if it fails.

For more information on configuring Supervisor and using Queues, consult
the <a href="https://laravel.com/docs/queues#running-the-queue-listener" target="_blank">Laravel Queue docs</a>.

### Application configuration

**Debug mode**

The debug setting is found in the `config/app.php` configuration file with the `debug` parameter, and is disabled by default.

When enabled, this setting will display detailed error messages when they occur along with other debugging functions.
Debug mode should always be disabled in a live production site. This prevents the display of potentially sensitive
information to the end user.

> **Important:** Always set the `APP_DEBUG` setting to false in production environments.

**CSRF protection**

TastyIgniter offers a simple method to protect your application from cross-site request forgeries.

For every active user session managed by the application, TastyIgniter automatically generates a CSRF "token." This
token is used to check that the authenticated user is the one who actually makes the client requests.

Although CSRF security is enabled by default, you can disable it by updating the `ENABLE_CSRF` variable in your application's `.env` file.

### Apache/Nginx configuration

TastyIgniter has basic configuration that should be applied to your webserver. Common webservers and their configuration
can be found below.

#### Apache configuration

TastyIgniter includes a [`.htaccess`](https://github.com/tastyigniter/TastyIgniter/blob/master/public/.htaccess) file - make sure it's been uploaded correctly. The `.htaccess` file is located in the `public` directory of your TastyIgniter installation. There are some extra system requirements if your webserver is running Apache, `mod_rewrite` should be installed and enabled and the `AllowOverride` option should be switched on.

```apache
<Directory "/path/to/tastyigniter/public">
    AllowOverride All
</Directory>
```

You will need to uncomment this line in the `.htaccess` file in some cases:

```html
## !IMPORTANT! You may need to uncomment the following line for some hosting environments,
## If your installation resides in a subdirectory, enter the name here also
##
# RewriteBase /
```

If you've created a subdirectory, you can specify the subdirectory name as well:

```html
RewriteBase /subdirectory/
```

#### Nginx configuration

Make sure that the [`.nginx.conf`](https://github.com/tastyigniter/TastyIgniter/blob/master/.nginx.conf) file included with TastyIgniter has been uploaded correctly. The `.nginx.conf` file is located in the root directory of your TastyIgniter installation. Then, assuming you have Nginx setup, add the following to your server's configuration block:

```html
include /path/to/tastyigniter/.nginx.conf;
```

As an example, your site conf file should look something like:

```html
server {
    listen 80;
    
    root /path/to/tastyigniter;
    index index.php;
    
    server_name mytastysite.com;
    
    gzip             on;
    gzip_proxied     expired no-cache no-store private auth;
    gzip_types       text/plain text/css application/x-javascript application/json application/javascript image/x-icon image/png image/gif image/jpeg image/svg+xml;
    
    charset utf-8;
    
    access_log off;
    
    include /path/to/tastyigniter/.nginx.conf;
}
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

> **Note:** A detailed log can be found in the `storage/logs/laravel.log` file.

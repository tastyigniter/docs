---
layout: page
title: "Installation"
---

Before you proceed with installing TastyIgniter, you should check that your server meets the minimum system requirements.

## Minimum Requirements

These are the requirements for your  web hosting to run TastyIgniter:

- PHP (at least 7.1)
- PDO PHP Extension
- cURL PHP Extension
- OpenSSL PHP Extension
- GD PHP Extension
- Mcrypt PHP Extension
- Mbstrings PHP Extension

## Install TastyIgniter

There are two ways you can install TastyIgniter, either using the Quick or Command-line installation instructions. 

### Quick Installation

1. [Download](https://tastyigniter.com/download) and unzip the TastyIgniter setup wizard into an empty directory on your server.
2. Create a MySQL user database for TastyIgniter on your database server.
3. Upload the TastyIgniter folders and files to your server. Normally the setup.php file will be at the web root directory.
4. Grant write permissions on the setup directory, its subdirectories and files.
4. Run the TastyIgniter setup script by accessing setup.php in your web browser. Example, http://example.com/setup.php or http://example.com/folder/setup.php
5. Follow all onscreen instructions and make sure all installation requirements are checked.

### Command-line installation

TastyIgniter also uses <a href="https://getcomposer.org/" target="_blank">composer</a> to manage its dependencies, you'll need to have composer installed on your machine before using the TastyIgniter command-line installation. Run this command in an empty location that you want TastyIgniter to be installed in:

```bash
composer create-project tastyigniter/tastyigniter:dev-master .
```

After running the above command, run the install command and follow the instructions to complete installation

```bash
php artisan igniter:install
```

The install command will guide you through the process of setting up TastyIgniter for the first time. It will ask for the database configuration, application URL and administrator details.

## Post-installation steps

For security reasons, if you used the [Quick Installation Setup Wizard](#quick-installation), you should delete the setup files. TastyIgniter will never automatically delete files from your system, so these files and directories should be deleted manually: 

```yaml
setup/      		<== Setup directory
setup.php       <== Setup script
```

### Setting up the task scheduler

You should add the following Cron entry to your server for scheduled tasks to function properly. Crontab editing is usually done with the command `crontab -e`. 

```
* * * * * php /path/to/artisan schedule:run >> /dev/null 2>&1
```

Be sure to replace `/path/to/artisan` with the absolute path to the artisan file in your TastyIgniter root directory . This Cron will call the command scheduler every minute. When executing the `schedule:run` command, TastyIgniter will assess your scheduled tasks and run the tasks that are due. 

> Task Scheduling is how scheduling time-based tasks are managed in TastyIgniter. Several core features of TastyIgniter, such as checking for updates, use the scheduler. 

### Setting up queue workers

Optionally, you can set up an external queue to process **queued jobs**, which will be handled by the platform asynchronously by default. By setting the default parameter in the `config/queue.php`, this behaviour can be altered. 

## URL Rewriting

### Apache

TastyIgniter includes a `.htaccess` file – make sure it’s been uploaded correctly. If you’re using shared hosting, confirm with your hosting provider that `mod_rewrite` is enabled. You may need to add the following to your Apache configuration:

```
<Directory "/path/to/your/tastyigniter">
    AllowOverride All
</Directory>
```

### Nginx

Add the following lines to your server’s configuration block:

```html
/etc/nginx/sites-available/default
```

Use the following code in **server** section. If you have installed TastyIgniter into a subdirectory, replace the first `/` in location directives with the directory TastyIgniter was installed under:

```html
location / { try_files $uri $uri/ /index.php?$query_string; }

# Pass the PHP scripts to FastCGI server
location ~ ^/index.php {
    # Write your FPM configuration here

}
```

## Getting Started

You can access the administrator panel from `/admin` with your username and password asked during the setup process. After you've logged in you'll be able to access the administration panel to configure your site.

> Follow the getting started steps on the administration panel dashboard.

## Getting help

- If you believe you have found a bug, please report it using the <a href="https://github.com/tastyigniter/TastyIgniter/issues" target="_blank">GitHub issue tracker</a>, or better yet, fork the repo and submit a pull request.
- If you have feedback to share, ideas you would like implemented, by all means, share them on the <a href="https://forum.tastyigniter.com" target="_blank">TastyIgniter Community Forums</a>.
- <a href="http://slack.tastyigniter.com/" target="_blank">Join us on our Slack channel</a> to chat with us.

## Troubleshooting
1. **A 404 error page is displayed:** This could be a result of the mod_rewrite module not being activated/installed or configured properly. Activate mod_rewrite for the Apache web-server.
If it is already activated, check the root htaccess file in `/.htaccess`, to make sure the `RewriteBase` value is configured properly.
2. **A blank screen is displayed when opening the application:** Check the file permissions are set correctly on the files and folders to 777.
3. **Setup successful but storefront links are not working:** Check that the theme's required extensions are all installed.

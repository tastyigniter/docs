---
layout: page
title: "Installation"
---

There are two ways you can install TastyIgniter, either using the Quick or Command-line installation instructions. Before you proceed, you should check that your server meets the minimum system requirements.

## Minimum Requirements

These are the requirements for your  web hosting to run TastyIgniter:

- PHP (at least 7.0)
- MySQL version 5.5 or higher
- PDO PHP Extension
- cURL PHP Extension
- OpenSSL PHP Extension
- GD PHP Extension
- Mcrypt PHP Extension
- Mbstrings PHP Extension

## Install TastyIgniter

### Quick Installation:

1. [Download]({{site.siteUrl}}/download) and unzip the TastyIgniter package into an empty directory on your server.
2. Create a MySQL user database for TastyIgniter on your web server.
3. Upload the TastyIgniter folders and files to your server. Normally the setup.php file will be at the web root directory.
4. Grant write permissions on the setup directory, its subdirectories and files.
4. Run the TastyIgniter setup script by accessing setup.php in your web browser. Example, http://example.com/setup.php or http://example.com/folder/setup.php
5. Follow all onscreen instructions and make sure all installation requirements are checked.

### Command-line installation

```
    composer create-project tastyigniter/tastyigniter mysite
```

### Enjoy

You can now login on /admin/login with your username and password asked during the setup process. After you've logged in you'll be able to access the administration panel to configure your site.

## Getting help

- If you believe you have found a bug, please report it using the [GitHub issue tracker](https://github.com/tastyigniter/TastyIgniter/issues), or better yet, fork the repo and submit a pull request.
- If you have feedback to share, ideas you would like implemented, by all means share them on the [TastyIgniter Community Forums](https://forum.tastyigniter.com).
- [Join us on our Slack channel](http://slack.tastyigniter.com/) to chat with us.

## Troubleshooting
1. **A 404 error page is displayed:** This could be a result of the mod_rewrite module not being activated/installed or configured properly. Activate mod_rewrite for the Apache web-server.
If its already activated, check the root htaccess file in `/.htaccess`, to make sure the `RewriteBase` value is configured poroperly.
2. **A blank screen is displayed when opening the application:** Check the file permissions are set correctly on the files and folders to 777.
3. **Setup successful but storefront links are not working:** Check that the theme's required extensions are all installed.

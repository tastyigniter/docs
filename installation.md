---
layout: page
section: user-guide
title: "Installation"
---

# Installation

## Quick Installation:

1. Download and unzip the TastyIgniter package, if you haven't already.
2. Create a MySQL database for TastyIgniter on your web server, as well as a MySQL user who has all access and modify privileges.
3. (Optional) Open the `database.php` file inside `system/tastyigniter/config`, then edit the file and add your database information.
4. Upload the TastyIgniter folders and files to your server. Normally the index.php file will be at your root.
5. Run the TastyIgniter installation script by accessing the URL in a web browser. This should be the URL where you uploaded the TasyIgniter files. Example, http://example.com/ or http://example.com/folder
6. Follow all onscreen informations and make sure all installation requirements are checked.
7. That’s it! TastyIgniter should now be installed.

## Troubleshooting
1. **A 404 error page is displayed:** This could be a result of the mod_rewrite module not being activated/installed or configured properly. Activate mod_rewrite for the Apache web-server.
If its already activated, check the htaccess file in `admin/.htaccess`, `main\.htaccess` and `setup\.htaccess` to make sure the `RewriteBase` value is configured poroperly.
2. **A blank screen is displayed when opening the application:** Check the permissions are set correctly on the files and folders.
3. **Setup successful but storefront links are not working:** Check the `main\.htaccess` file to make sure `RewriteBase` value is properly configured. If your installation resides in a sub-folder, set the value accordingly.

---
title: "Upgrade Guide"
section: "getting-started"
sortOrder: 30
---

> As of TastyIgniter 2.0 or later, When a new version of TastyIgniter is available you will receive an update message in your TastyIgniter Admin Panel. All you have to do to update TastyIgniter is to click the **Update** notification button at the right top. After that you will be redirected to the **Update Center** page.

There are two methods for updating - the easiest is through the **Update Center**, which will work in most cases. 
If it doesn't work, or you just prefer to be more hands-on, you can follow the manual update process.

## Upgrading TastyIgniter v2.1.x to v3.x.x

This is a huge release. It contains a massive change in the codebase frrom CodeIgniter to Laravel with some new features and plenty bug fixes.

Listing all the improvements in v3 would be great but I hardly kept track. So straight to upgrade!



## Upgrading TastyIgniter v2.0.x to v2.1.x

{{alerts.callout_info}}After UPDATE make sure your layout modules are displaying on the storefront. You can use the new layout modules drag and drop under Design > Layouts.{{alerts.end}} 

## Back up TastyIgniter

Before you get started, it's a good idea to back up your website. This means if there are any issues you can restore your website. 

### Backing Up Your Database
Visit your TastyIgniter admin page at /admin. Go to `Maintenance` under `System -> Tools` and `Select tables to backup` then click the `Backup` button. On the next page, make sure `Add DROP TABLE statement` is `No`, click the `Backup` button again and your backed up database file will appear under the `Exisiting Backups` Tab. You can download and keep the file on your computer.

### Backing Up Your TastyIgniter Site
Backup your files using FTP clients or cPanel filemanager to copy or create a zip of all the existing TastyIgniter files and folders.

{{alerts.warning}}The upgrade process will affect all files and folders included in the TastyIgniter installation. This includes all the core files used to run TastyIgniter. If you have made any modifications to those files, your changes will be lost.{{alerts.end}} 

### Restoring Your Database From Backup
Visit your TastyIgniter admin maintenance page. Under `Exisiting Backups` tab click the `Restore` button next to the database backup you wish to restore.

## Manual Update

If the one-click upgrade doesn't work for you, don't panic! Just try a manual update.

### **Step 1:** Replace TastyIgniter files
1. Get the [latest TastyIgniter](https://tastyigniter.com/download) zip file
2. Unpack the downloaded zip file
3. Open the unpacked folder `TastyIgniter-2.x.x`
4. Locate and `delete` the `database.php` file in `system/tastyigniter/config/` folder
5. Using your FTP client, upload all the files and folders inside the `TastyIgniter-2.x.x` folder to your web host. Uploading all the files might take a few minutes on the FTP client.
6. Do NOT replace/overwrite your existing `system/tastyigniter/config/database.php` file.

### **Step 2:** Update your installation
After uploading the files of the new version to your web host, visit the setup page at "/setup" like: www.myrestaurant.com/setup. Following the instructions on the setup page will update your database to be compatible with the latest code.

{{alerts.warning}}Warning: DO NOT proceed if you see the Database page asking you to enter your database details, this means your old database.php file has changed. Fix this by restoring your old database.php file then return to Step 1{{alerts.end}}


### **Step 3:** Do something nice for yourself
Clear your site (if enabled) and browser cache at this point so the changes will go live immediately. Otherwise, visitors to your site (including you) will continue to see the old version (until the cache updates).

Your TastyIgniter installation is successfully updated..

{{alerts.warning}}If you experience any issue, you should restore your most recent database & file backup and try again{{alerts.end}}

{{alerts.note}}THIS IS FOR UPGRADE ON EXISTING INSTALLS ONLY! IF INSTALLING NEW, BE SURE TO READ THE README.md FILE INSTEAD{{alerts.end}}


## Upgrading TastyIgniter v1.4.x to 2.0.x

1. BACKUP YOUR EXISTING STORE FILES AND DATABASE!!
    - Backup your database via your store `Admin->Tools->Maintenance->Backup`
    - Backup your files using FTP file copy or use cPanel filemanager to create a zip of all the existing tastyigniter files and folders

2. Download the latest version of TastyIgniter and upload ALL new files on top of your current install EXCEPT your `system/tastyigniter/config/database.php`.
    - Make sure your `system/tastyigniter/config/database.php` old file was not overwritten.

3. Go to http://myrestaurant.com/ Replacing myrestaurant.com with your actual site (and subdirectory if applicable).

4. You should see the TastyIgniter Setup script.

5. Click **Continue**. After a few seconds you should see the installation success page.
    - If you see the database configuration and/or site settings TastyIgniter Setup page, then that means you have replaced your old `system/tastyigniter/config/database.php` file. Restore them from your backup first. Then try again.

6. Clear any cookies in your browser

7. Go to the administrator panel and login as the main administrator. Press Ctrl+F5 3x times to refresh your browser cache. That will prevent oddly shifted elements due to stylesheet changes.
    - If you see any errors, report them immediately in the forum before continuing.

9. Go to `Admin->System` Settings
    - Update any blank fields and click save.
    - Even if you do not see any new fields, click save anyway to update the database with any new field names.


## Troubleshooting

If you have any upgrade script errors, post them in the forum
You should always visit the forum immediately after a fresh upgrade to see if there are any immediate bug fixes
If nobody has reported your bug, then please report it.
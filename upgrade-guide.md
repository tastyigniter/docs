---
layout: page
title: "Upgrade Guide"
callout: This section is incomplete. Please help to improve it.
---

# Upgrade Guide

{{site.data.alerts.callout_primary | markdownify }} You should always update TastyIgniter to the [latest version]({{site.siteurl}}/download). {{site.data.alerts.end}}

{{site.data.alerts.warning}}The upgrade process will affect all files and folders included in the TastyIgniter installation. This includes all the core files used to run TastyIgniter. If you have made any modifications to those files, your changes will be lost.{{site.data.alerts.end}} 

As of TastyIgniter 2.0 or later, When a new version of TastyIgniter is available you will receive an update message in your TastyIgniter Admin Panel. 
All you have to do to update TastyIgniter is to click the **Update** notification button at the right top. 
After that you will be redirected to the **Update Center** page.

There are two methods for updating - the easiest is through the **Update Center**, which will work in most cases. 
If it doesn't work, or you just prefer to be more hands-on, you can follow the manual update process.

## Back up TastyIgniter

Before you get started, it's a good idea to back up your website. This means if there are any issues you can easily restore your website. 

### Backing Up Your Database
### Backing Up Your TastyIgniter Site
### Restoring Your Database From Backup

## Manual Update

If the one-click upgrade doesn't work for you, don't panic! Just try a manual update.

### **Step 1:** Replace TastyIgniter files
### **Step 2:** Update your installation
### **Step 3:** Do something nice for yourself

## Upgrading TastyIgniter v1.4.x to 2.0.x

{{site.data.alerts.note}}THIS IS FOR UPGRADE ON EXISTING INSTALLS ONLY! IF INSTALLING NEW, BE SURE TO READ THE README.md FILE INSTEAD{{site.data.alerts.end}}

1. BACKUP YOUR EXISTING STORE FILES AND DATABASE!!
    - Backup your database via your store `Admin->Tools->Maintenance->Backup`
    - Backup your files using FTP file copy or use cPanel filemanager to create a zip of all the existing tastyigniter files and folders

2. Download the latest version of TastyIgniter and upload ALL new files on top of your current install EXCEPT your `system/tastyigniter/config/database.php`.
    - Make sure your `system/tastyigniter/config/database.php` old file was not overwritten.

3. Go to http://yourstore.com/ Replacing yourstore.com with your actual site (and subdirectory if applicable).

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
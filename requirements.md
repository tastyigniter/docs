---
layout: page
title: "Requirements"
summary: When selecting a hosting company, you should check if the following requirements are provided
---

# Requirements

## Your Host Environment

There are three core requirements for your host to run TastyIgniter:

- Web Server (Apache recommended)
- PHP (at least 5.4)
- MySQL version 4.1 or higher (MySQLi recommended)

{{site.data.alerts.note}}TastyIgniter is compatible with almost every server that supports PHP and MySQL. However, we recommend Apache{{site.data.alerts.end}}

## Your Host Configuration Requirements
- Curl
- GD/GD2
- Mcrypt
- Mbstrings
- Register Globals = Off
- File Uploads = On

These requirements are provided by almost all hosting companies.

- **safe_mode, file_uploads, allow_url_fopen** PHP directives should be enabled
- **GD library** should be installed.
- **cURL** support should be enabled. You need this PHP extension to ensure support of secure connections, some payment systems such as PayPal and Authorize.Net.
- **.htaccess** file (if supported) should have the following directives allowed: DirectoryIndex, Deny, Allow, Options, Order, AddHandler, RewriteEngine, RewriteBase, RewriteCond, and RewriteRule

During the installation/upgrade process, TastyIgniter will check if you have them all and not let you continue if any is missing. In such case, you should contact your hosting provider to make them available.

{{site.data.alerts.note}}We recommend a Unix-like operating system for your server, such as FreeBSD, Linux, or OS X. These systems are scalable, more secure, and offer better performance.{{site.data.alerts.end}}
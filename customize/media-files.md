---
title: "Media files"
section: customize
sortOrder: 150
callout: This section is incomplete. Please help to improve it.
---

## Introduction

By default Media Manager works with the `/assets/media` directory. It is possible to use external storages such as Amazon S3 or Rackspace CDN.

> The [Drivers extension](http://tastyigniter.com/marketplace/item/igniter-drivers) must be installed before you can use Amazon S3 or Rackspace CDN.

Reset the Media Manager cache, once you change its configuration.

## Media configuration options

Define options under **assets.media** within the **config/system.php** file to configure the Media Manager according to your preference. 

Additional options can be configured from **System > Settings > Media** in the Admin interface.

For more information on configuring external storage, check out [laravel's filesystem docs](https://laravel.com/docs/filesystem#configuration).

## Working with Media files

Adding media in TastyIgniter is very easy. All of your media can be managed in the Media Manager. If a media file is uploaded within the edit screen, it will automatically be attached to the current menu item being edited. 
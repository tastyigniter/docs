---
title: "Localization"
section: advanced
sortOrder: 300
---

## Introduction

TastyIgniter has a powerful translation system that allows your site to support multilingual content. When developing your extension, you should consider using locale strings even if you do not intend to use it in more than one language. You never know: if you decide to make your extension available to users around the world, it can be handy later.

We strongly recommend including a complete set of English resources with each extension, also if you decide to publish your extension in the TastyIgniter marketplace.

Locale files are stored within the extension **/language** subdirectory.

## Directory structure

Here is an example of the extension `language` directory:

```yaml
extensions/
  igniter/
    demo/             				<=== Extension directory
      language/       				<=== Localization directory
        en/           				<=== Language directory
          default.php    				<=== Locale file
        es/
          default.php
```

> Each language directory should be named using the <a href="https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes" targer="_blank">ISO 639-1</a> code for the language it contains.

## Defining locale strings

All locale files return an array of keyed locale strings. For example:

```php
<?php

return [
    'sample_key' => 'This is a sample locale string.',
    'alert' => [		// Namespacing for alerts locale strings 
        'success' => 'This is a success alert'
    ]
];
```

## Accessing locale strings 

You can use the `lang` helper function to get strings from locale files. The method accepts the file and key of the locale string as its first argument. For example, let's the `sample_key` locale string from the locale file `extensions/igniter/demo/language/en/lang.php`:

```php
@lang('igniter.demo::lang.sample_key')

@lang('igniter.demo::lang.alert.success')
```

## Overriding locale strings

System users can override extension locale strings without altering the extension files to modify these strings. For
example, you should create the locale file `default.php` at the following location to override the locale
string `sample_key` within the `default.php` file of the `igniter/demo`  extension:

> You can also easily edit core and/or extension locale strings from the admin interface on the **Localization > Languages > Translations** page

```yaml
language/                 <=== Localization directory
  en/                     <=== Language directory
    igniter/
      demo/               <=== Extension directory
        default.php          <=== Locale override file
```

You should only define the locale strings you want to override in this file. Any locale strings you do not override are still loaded from the original locale files of the extension.

```php
<?php

return [
    'sample_key' => 'This is an overidden locale string.',
];
```

## Making your site multilingual

In this section of the article, you will find a complete overview of the steps involved in the creation of a multilingual TastyIgniter site.

### Manually download a Language Pack

Follow these steps to manually download a community translated language pack.

- Join our translations <a href="https://translate.tastyigniter.com/" target="_blank">TastyIgniter Community Translation project</a>.
- From the Browse all projects page, select the project you want to install a language pack for, such as **TastyIgniter**.
- Click on the **Languages** tab to view the available languages.
- You will see a list of languages with their translation progress.
- Click on the language you want to install. For example, let's choose **Spanish (ES)**.
- You will be redirected to the language page, where you can see the translation progress.
- Click on the **Files** tab to view the available download options for that particular language.
- Click on the **Download original translation files as ZIP file** option to download the language pack.
- The extracted zip file should have the following folder and file structure.

```yaml
tastyigniter/
  v4-admin/
    es/
      4.x/
        admin.php
  v4-main/
    es/
      4.x/
      main.php
  v4-system/
    es/
      4.x/
      system.php
```

- Copy the `v4-admin/es/4.x/admin.php`, `v4-main/es/4.x/main.php`, and `v4-system/es/4.x/system.php` files to your TastyIgniter `lang/vendor/igniter/es_ES/` directory, creating the necessary directories if they do not exist.

- The final directory structure should look like this:
  

```yaml
language/
  es_ES/                       <=== Language directory
    admin/
      lang.php                 <=== Locale file
    main/
    system/
    igniter/
      demo/
        default.php               <=== Locale file
```

> Notice the language directory name uses **underscore** instead of an hypen (e.g. “es_ES”).

### Installing a Language Pack

After downloading a language pack, you must create a new language in the admin interface to register the language into the system.

1. Create a new language from the **Localisation > Languages** page of the admin interface.
2. Fill in the form. The value of the `Locale Code` field must match the language directory name. Using the example above, the value will be `es_ES`
3. Lastly, toggle the **Status** switch to `Enabled` then save the form.

### Setting the Default Language

You may want to set the installed language pack as the default language for new users and visitors. You can do this on
the **System > Settings > General** page of the admin interface, under the **Site** tab select the language you want to
use as your default.

### Enabling language detection

TastyIgniter includes support for language negotiation out-the-box using a variety of methods without forcing the user to choose his or her language. The language is detected in the following sequence:

- Determine the language from the **request/session** parameter.
- Determine the language from the user browser's language settings.

## Third-Party Extensions

While language packs downloaded from the **Browse Languages** page of the admin interface may usually provide translations for all recommended extensions bundled with TastyIgniter, as a rule, they will not cover any third party extensions that you may have installed. Developers are responsible for providing and maintaining translations of their extensions.

So before you install a third-party extension, you should check to make sure it includes translations for each language pack you have installed. If you find that an extension doesn't support a language you need, please contact the developer directly and arrange to have the necessary translations added.

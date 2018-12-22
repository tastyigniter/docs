---
layout: page
section: extending
title: "Localization"
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
    demo/             <=== Extension directory
      language/       <=== Localization directory
        en/           <=== Language directory
          lang.php    <=== Locale file
        es/
          lang.php
```

> Each language directory should be named using the [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) code for the language it contains.

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
echo lang('igniter.demo::lang.sample_key');

echo lang('igniter.demo::lang.alert.success');
```

## Overriding locale strings

System users can override extension locale strings without altering the extension files to modify these strings. For example, you should create the locale file `lang.php` at the following location to override the locale string `sample_key` within the `lang.php` file of the `igniter/demo`  extension: 

```yaml
language/				<=== Localization directory
  en/					<=== Language directory
    igniter/
      demo/          	<=== Extension directory
        lang.php    	<=== Locale override file
```

You should only define the locale strings you want to override in this file. Any locale strings you do not override are still loaded from the original locale files of the extension.

```php
<?php

return [
    'sample_key' => 'This is an overidden locale string.',
];
```

## Making your site multilingual

Easily create a multilingual site using extensions developed by the TastyIgniter community. In this section of the article, you will find a complete overview of the steps involved in the creation of a multilingual TastyIgniter site.

### Installing a language pack

In the admin interface, you can easily install your desired language on the **Localization > Languages > Browse Languages** page.

### Setting the default admin language

You can select the default language for your site and the administrator interface after the installation is complete on the **Localization > Languages** page.

### Enabling language detection

TastyIgniter includes support for language negotiation out-the-box using a variety of methods without forcing the user to choose his or her language. The language is detected in the following sequence:

- Determine the language from the **request/session** parameter.
- Determine the language from the user browser's language settings.
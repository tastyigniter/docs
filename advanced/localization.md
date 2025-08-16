---
title: "Localization"
section: advanced
sortOrder: 310
---

## Introduction

TastyIgniter features a robust translation system that leverages Laravel's localization features, providing a convenient method for retrieving strings in various languages, allowing you to easily support multiple languages in your TastyIgniter application.

Language directories and files are stored in PHP files within your application's **lang** directory.

## Directory structure

Here is an example of how the **lang** directory might be structured:

```yaml
  lang/
    en/                     <=== Language directory
  custom.php                <=== Language file
    es/                     <=== Language directory
      custom.php            <=== Language file
```

In this example, the language directory contains language files. Each language has its own subdirectory named using the [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) code (e.g., `en` for English, `es` for Spanish), and each subdirectory contains a `custom.php` language file with the translation strings for that language.

## Defining language strings

All language files return an array of keyed language strings. For example:

```php
<?php

return [
    'sample_key' => 'This is a sample language string.',
    'alert' => [  // Namespacing for alerts language strings 
        'success' => 'This is a success alert'
    ]
];
```

## Accessing language strings

You can use the `Lang` facade, `@lang` Blade directive, `__` helper function, or `lang` helper function to retrieve strings from language files. For instance, to retrieve the `sample_key` language string from the `lang/en/custom.php` language file, you can use any of these methods.

```php
// Using the `Lang` facade
echo Lang('custom.sample_key')

// Using the `@lang` Blade directive
@lang('custom.sample_key')

// Using the `lang` helper function
echo lang('custom.sample_key')

// Using the `lang` helper function in a Blade template
{{ lang('custom.sample_key') }}

// Using the `__` helper function
echo __('custom.sample_key');

// Using the `__` helper function in a Blade template
{{ __('custom.sample_key') }}
```

You can also retrieve nested language strings using dot notation. For example, to retrieve the `success` language string from the `alert` namespace in the `lang/en/custom.php` language file:

```php
// Using the `Lang` facade
{{ __('custom.alert.success') }}
```

### Replacing parameters in translation strings

If you wish, you can define placeholders in your language strings. All placeholders are prefixed with a `:`. For example, you can define a welcome message with a placeholder name like so:

```php
<?php

return [
    'welcome' => 'Welcome, :name',
];
```

To replace the placeholders when retrieving a language string, you may pass an array of replacements as the second argument to the `__` function:

```php
{{ __('custom.welcome', ['name' => 'dayle']) }}
```

If your placeholder contains all capital letters, or only has its first letter capitalized, the translated value will be capitalized accordingly:

```php
<?php

return [
    'welcome' => 'Welcome, :NAME', // Welcome, DAYLE
    'goodbye' => 'Goodbye, :Name', // Goodbye, Dayle
];
```

### Pluralization with language strings

Pluralization can be a complex issue due to the various rules across different languages. However, Laravel provides a solution to translate strings differently based on your defined pluralization rules. You can distinguish between singular and plural forms of a string using the `|` character, like this:

```php
'apples' => 'There is one apple|There are many apples',
```

You can create even more complex pluralization rules, specifying translation strings for multiple value ranges:

```php
'apples' => '{0} There are none|[1,19] There are some|[20,*] There are many',
```

Once you've defined a translation string with pluralization options, you can use the `trans_choice` function to retrieve the line for a given "count". For example:

```php
{{ trans_choice('custom.apples', 10) }}
```

You can also define placeholder attributes in pluralization strings. These placeholders can be replaced by passing an array as the third argument to the `trans_choice` function:

```php
'minutes_ago' => '{1} :value minute ago|[2,*] :value minutes ago',
 
{{ trans_choice('custom.minutes_ago', 5, ['value' => 5]) }} // 5 minutes ago
```

If you want to display the integer value passed to the `trans_choice` function, you can use the built-in `:count` placeholder:

```php
'apples' => '{0} There are none|{1} There is one|[2,*] There are :count',

{{ trans_choice('custom.minutes_ago', 5, ['value' => 5]) }} // There are 5
```

## Overriding language strings

You can customize all application language strings from the _Manage > Settings > Languages > Edit > Translations_ admin page.

Some TastyIgniter extensions, themes and Laravel packages come with their own language files. If you need to customize these strings without modifying the package's core files, you can override them by placing files in the `lang/vendor/{author}-{package}/{locale}` directory.

For instance, if you want to modify the language string `override_key` in `custom.php` for an extension named `acme.helloworld`, you should create a language file `lang/vendor/acme-helloworld/en/custom.php` within the root of your TastyIgniter installation.

```yaml
lang/
  vendor/ 
    acme-helloworld/ 
      en/                     <=== Language directory
        custom.php            <=== Language override file
```

In this file, define only the language strings you want to change. Any strings you don't override will still be loaded from the original language file.

```php
<?php

return [
    'sample_key' => 'This is an overridden language string.',
];
```

## Making your site multilingual

In this section, we'll guide you through the steps required to make your TastyIgniter site multilingual. There are two main ways to install additional languages: directly from the _Manage > Settings > Languages_ page in the admin interface, or manually downloading a language pack from the TastyIgniter translations <a href="https://translate.tastyigniter.com/" target="_blank">TastyIgniter Community Translation project website</a>.

### Importing translated language strings

TastyIgniter supports the use of language packs, which are collections of translated language strings for various languages. These language packs can be imported to provide translations for the TastyIgniter admin interface and frontend.

Language packs are typically provided by the TastyIgniter community and can be installed directly from the admin interface or manually downloaded from the TastyIgniter translations <a href="https://translate.tastyigniter.com/" target="_blank">TastyIgniter Community Translation project website</a>.

You can import translated language strings from the _Manage > Settings > Languages_ page of the admin interface.

- Navigate to the _Manage > Settings > Languages_ page in the admin interface.
- Click on the **New** button to create a new language. In the **Locale Code** field, enter the language code (e.g., `es` for Spanish) and the **Name** field, enter the language name (e.g., `Spanish`).
- Click the **Save** button to create the new language.
- Once the language is created, you will see the **Import Translations** button on the right side of the page.
- Click on the **Import Translations** button to open the import dialog.
- In the import dialog, you will see a list of available language packs.
- Click on the **Download translated strings** button to download the language packs.
- Once the download is complete, click on the **Close** button to reload the page and see the newly installed translated language strings
- Go to the _Manage > Settings > Languages_ page and click on the **Set as Default** button next to the language you just created to activate it.
- That's it! You have successfully installed a new language pack.

#### Installing a language pack from the command line

You can also install a language pack from the command line using the `igniter:language-install` Artisan command. This command allows you to install a language pack directly from the TastyIgniter Community Translation project.

Run the following command from the application directory to install a **Spanish (ES)** language pack:

```bash
php artisan igniter:language-install es
```

### Manually download a language pack

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
lang/
  vendor/
    igniter/
      es_ES/                       <=== Language directory
        admin.php
        main.php
        system.php
```

> Notice the language directory name uses underscore `_` instead of a hyphen `-` (e.g. "es_ES").

### Configuring the default language

You may want to set the installed language pack as the default language for new users and visitors. You can do this on the _Manage > Settings > Languages_ page of the admin interface, by clicking the **Set as Default** button next to the language you want to use as the default.

### Enabling language detection

TastyIgniter can automatically detect and set the user's preferred language using several methods. The detection order differs slightly between the Admin Interface and the Frontend:

- **Admin Interface**
  1. Uses the language set in the currently logged-in admin user's profile.
  2. If not set, falls back to the application's default language setting.

- **Frontend**
  1. Checks for a language preference in the request or session (such as a URL parameter or previously selected language).
  2. If not found, uses the application's default language setting.

This automatic detection helps ensure users see the site in their preferred language whenever possible.

## Translating third-party extensions

Language packs downloaded from the _Manage > Settings > Languages_ page in the admin interface typically include translations for all TastyIgniter recommended extensions. However, it's important to note that they may be missing translations for other extensions you've installed.

Developers of TastyIgniter extensions are responsible for providing and maintaining translations for their extensions. Before installing a third-party extension, ensure that it includes translations for each language pack you have installed. If you find that an extension doesn't support a language you need, please contact the developer directly to arrange for the necessary translations to be added.

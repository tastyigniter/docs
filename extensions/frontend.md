---
title: "Frontend Extension"
section: "extensions"
sortOrder: 50
---

## Introduction

Add more functionality to your TastyIgniter website with these awesome basic tools.

## Features
- Place banners on any front-end page
- Manage and display featured menu items
- Display a subscribe form to gain subscribers using mailchimp
- Manage and show an hero slider on any front-end page
- Contact form
- Display Google reCAPTCHA on any front-end page

> When adding components to a page or layout, use the admin user interface to avoid common errors.

## Installation

To install this extension, click on the **Add to Site** button on the TastyIgniter marketplace item page or search for **Igniter.Frontend** in **Admin System > Updates > Browse Extensions**

## Admin Panel

In the admin user interface you can manage banners, featured items and images for the slider.
Manage banners on the **Design > Banners & Sliders** page and slider on the **System > Settings > Slider Settings** page

## Captcha Configuration

In the admin go to **Settings > reCaptcha Settings**. You must enter a `Site Key` and a `Secret Key`.
Selecting a language is optional. Please follow the instructions on the settings page to get these keys.

## Components

| Name     | Page variable                | Description                                      |
| -------- | ---------------------------- | ------------------------------------------------ |
| Banners  | `@component('banners')` | Displays banners |
| Contact | `@component('contact')` | Displays Contact form              |
| FeaturedItems | `@component('featuredItems')` | Displays featured menu items               |
| Newsletter | `@component('newsletter')` | Displays newsletter subscribe form               |
| Slider | `@component('slider')` | Displays carousel slider              |
| Captcha | `@component('captcha')` | Displays Google reCAPTCHA             |

### Banners Component

**Properties**

| Property                 | Description              | Example Value | Default Value |
| ------------------------ | ------------------------ | ------------- | ------------- |
| banner_id                 | Banner ID          |        |         |
| width                     | Width            | 960        | 960         |
| height                     | Height            | 360        | 360         |

**Variables available in templates**

| Variable                  | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `{{ $banner }}` | The banner to display                                         |

**Example:**

```
---
'[banners]':
    width: 960
    height: 360
---
...
@component('banners')
...
```

**Example of multiple banners on a single page:**

```
---
'[banners bannerOne]':
    banner_id: 1
    width: 960
    height: 360

'[banners bannerTwo]':
    banner_id: 2
    width: 960
    height: 360

'[banners bannerThree]':
    banner_id: 3
    width: 960
    height: 360
---
...
@component('bannerOne')
@component('bannerTwo')
@component('bannerThree')
...
```

### Contact Component

**Properties**

| Property                 | Description              | Example Value | Default Value |
| ------------------------ | ------------------------ | ------------- | ------------- |
| redirectPage                 | Page path to redirect to after contact form has been sent successfully     |   contact     |   contact      |

**Example:**

```
---
title: 'Contact'
permalink: /contact

'[contact]':
---
...
@component('contact')
...
```

### Featured Items Component

**Properties**

| Property                 | Description              | Example Value | Default Value |
| ------------------------ | ------------------------ | ------------- | ------------- |
| title                 | Title to display          |        |         |
| items                 | List of menu item ids to display |        |         |
| limit                     | Number of items to display            | 12        | 12         |
| itemsPerRow                     | Number if items to display per row            | 3        | 3         |
| itemWidth                     | Item Width            | 400        | 400         |
| itemHeight                     | Item Height            | 300        | 300         |

**Variables available in templates**

| Variable                  | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `{{ $featuredTitle }}` | Title                                               |
| `{{ $featuredPerRow }}` | Items per row                                               |
| `{{ $featuredWidth }}` | Item width                                              |
| `{{ $featuredHeight }}` | Item height                                                 |
| `{{ $featuredMenuItems }}` | List of featured items to display                                   |

**Example:**

```
---
title: 'Home'
permalink: /

'[featuredItems]':
    items: ['1', '2', '3', '4']
    limit: 3
    itemsPerRow: 3
    itemWidth: 400
    itemHeight: 300
---
...
@component('featuredItems')
...
```

### Newsletter Component

**Example:**

```
---
'[newsletter]': {  }
---
...
@component('newsletter')
...
```

### Slider Component

**Example:**

```
---
'[slider]': {  }
---
...
@component('slider')
...
```

### Captcha Component

Using the captcha is as easy as possible: simply place the captcha component anywhere between `< form >` and `</form >` on your pages. Of course, you can also use the captcha component on multiple pages.

**Example:**

```
---
'[captcha]': {  }
---
<form>
...
@component('captcha')
...
</form>
```

**Verify submitted data**

Add recaptcha to your rules

```
$validator = Validator::make(request()->all(), [
    ...
    'g-recaptcha-response' => 'recaptcha',
]);
```

Print form errors

```
$errors = $validator->errors();
```
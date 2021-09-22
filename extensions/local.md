---
title: "Local Extension"
section: "extensions"
sortOrder: 60
---

## Introduction

## Features:
- Search nearby location
- Delivery areas (zones) boundary
- Location opening hours
- Component for displaying Opening Hours
- Component for displaying Menu Items
- Component for displaying Menu Categories
- Component for displaying Order Reviews

## Installation

To install this extension, click on the **Add to Site** button on the TastyIgniter marketplace item page or search for **Igniter.Local** in **Admin System > Updates > Browse Extensions**

## Admin Panel
Manage locations and menu items from the admin panel.

## Components
| Name     | Page variable                | Description                                      |
| -------- | ---------------------------- | ------------------------------------------------ |
| Search  | `@component('localSearch')` | Display the nearby search box on the page |
| LocalBox  | `@component('localBox')` | Display information about and manages the user's location |
| Info  | `@component('localInfo')` | Display the opening hours of the user's location |
| List  | `@component('localBox')` | Display a list of locations on the page |
| Menu  | `@component('localMenu')` | Display a list of menu items on the page |
| Categories | `@component('categories')` | Displays menu categories on the page            |
| Review  | `@component('localReview')` | Display a list of reviews on the page |
| Gallery | `@component('gallery')` | Displays the location gallery on the page            |

### Search Component

**Properties**

| Property                 | Description              | Example Value | Default Value |
| ------------------------ | ------------------------ | ------------- | ------------- |
| hideSearch                     | Hides the search field            | true/false        | false         |
| menusPage                     | Page name to the menus page            | local/menus         | local/menus         |

**Variables available in templates**

| Variable                  | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `{{ $searchQueryPosition }}` | Instance of the search query coordinates |

**Example:**

```
---
title: 'Home'
permalink: /

'[localSearch]':
    hideSearch: 0
    menusPage: local/menus
---
...
@component('localSearch')
...
```

### LocalBox Component

**Properties**

| Property                 | Description              | Example Value | Default Value |
| ------------------------ | ------------------------ | ------------- | ------------- |
| paramFrom           | URL routing code used for determining the location slug           | location        | location         |
| redirect           | Page name to redirect to when location is not loaded            | home        | home         |
| defaultOrderType           | The default selected order type            | true/false        | false         |
| showLocalThumb           | Show/hide the current location's thumb            | true/false        | false         |
| locationThumbWidth        | Location thumb Height        |        80           |      80    |
| locationThumbHeight       | Location thumb Width     |        80           |      80    |
| menusPage           | Page name to the menus page           |    local/menus     |     local/menus     |
| cartBoxAlias            | Used internally to find the CartBox component   |    cartBox     |     cartBox    |

**Variables available in templates**

| Variable                  | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `{{ $location }}` | Instance of the location class                                                |
| `{{ $locationCurrent }}` | Instance of the current location model                                                |
| `{{ $locationTimeslot }}` | Delivery/Pick-up schedule order timeslot                                         |
| `{{ $locationCurrentSchedule }}` | Instance of the current location schedule (delivery or collection)                                                |

**Example:**

```
---
title: 'Menus'
permalink: '/:location?local/menus/:category?'
...

'[localBox]':
    paramFrom: location
    showLocalThumb: 0
    menusPage: local/menus
---
...
@component('localBox')
...
```

### LocalInfo Component

**Properties**

| Property                 | Description              | Example Value | Default Value |
| ------------------------ | ------------------------ | ------------- | ------------- |

**Variables available in templates**

| Variable                  | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `{{ $locationInfo }}` | Data object of the current location                                                |

**Example:**

```
---
title: 'Menus'
permalink: '/:location?local/info'
...

'[localInfo]': { }
---
...
@component('localInfo')
...
```

### LocalList Component

**Properties**

| Property                 | Description              | Example Value | Default Value |
| ------------------------ | ------------------------ | ------------- | ------------- |
| distanceUnit           | Distance unit to use   | mi/km        | mi         |

**Variables available in templates**

| Variable                  | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `{{ $distanceUnit }}` | Unit of length to use for the distance. Uses system settings value                                               |
| `{{ $filterSearch }}` | The user's search query                                                |
| `{{ $filterSorted }}` | The user's selected filter                                                |
| `{{ $filterSorters }}` | Array of available filters                                                |
| `{{ $userPosition }}` | Instance of the user current position                                                |
| `{{ $locationsList }}` | List of location matching the search and/or filters                                         |

**Example:**

```
---
title: 'Locations'
permalink: '/locations'
...

'[localList]': { }
---
...
@component('localList')
...
```

### LocalMenu Component

**Properties**

| Property                 | Description              | Example Value | Default Value |
| ------------------------ | ------------------------ | ------------- | ------------- |
| isGrouped         | Number of menu items per page           | 50        | 20         |
| menusPerPage    | Number of menu items per page           | 50        | 20         |
| showMenuImages    | Show/hide menu item images            | true/false        | false         |
| menuImageWidth    | Width of the menu item image            | 90        | 90         |
| menuImageHeight    | Height of the menu item image            | 85        | 85         |
| defaultLocationParam         | The default location route parameter (used internally when no location is selected) | local   |   local   |
| localNotFoundPage            | lang:igniter.local::default.label_redirect | home |   home    |
| hideMenuSearch           | Hide the menu item search form | true/false |   true    |
| forceRedirect            | Whether to force a page redirect when no location param is present in the request URI. | true/false |   true    |

**Variables available in templates**

| Variable                  | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `{{ $menuSearchTerm }}` |                                                |
| `{{ $menuList }}`     | List of menu items                                              |

**Example:**

```
---
title: 'Menus'
permalink: '/:location?local/menus/:category?'
...

'[localMenu]':
    menusPerPage: 200
    showMenuImages: 0
    menuImageWidth: 95
    menuImageHeight: 80
---
...
@component('localMenu')
...
```

### Categories Component

**Properties**

| Property                 | Description              | Example Value | Default Value |
| ------------------------ | ------------------------ | ------------- | ------------- |
| menusPage | Page name of the menus page         |            |       |
| hideEmptyCategory | Hide categories with no items from the list         |            |       |
| hiddenCategories | Categories to hide from the list         |            |       |

**Variables available in templates**

| Variable                  | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `{{ $categories }}` | Value holds the categories tree                                               |
| `{{ $selectedCategory }}` | The user selected category                                               |

**Example:**

```
---
title: 'Menus'
permalink: '/:location?local/menus/:category?'
...

'[categories]':
    menusPage: local/menus
---
...
@component('categories')
...
```

### LocalReviews Component

**Properties**

| Property                 | Description              | Example Value | Default Value |
| ------------------------ | ------------------------ | ------------- | ------------- |
| pageLimit | Number of reviews per page       |   20    |     20      |
| sort | Sort the review list             |    created_at asc    |     created_at asc      |
| reviewableType | Whether the review form is loaded on an order or reservation page, use by the review form            |   order  |     order      |
| reviewableHash | Review sale identifier(hash), use by the review form            |   {{ :hash }}  |     {{ :hash }}      |
| redirectPage | Page name to redirect to when reviews is disabled       |   local/menus  |     local/menus      |

**Variables available in templates**

| Variable                  | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `{{ $reviewRatingHints }}` | Array of rating hints. Ex. Good, Bad, ...          |
| `{{ $reviewList }}` | List of reviews to display                                              |

**Example:**

```
---
title: 'Locations'
permalink: '/:location?local/reviews'
...

'[localReview]':
    pageLimit: 10
    sort: 'created_at asc'
---
...
@component('localReview')
...
```

### LocalGallery Component

**Variables available in templates**

| Variable                  | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `{{ $gallery }}` | List of location gallery images                                         |

**Example:**

```
---
title: 'Locations'
permalink: /:location?local/gallery
...

'[localGallery]':
---
...
@component('localGallery')
...
```
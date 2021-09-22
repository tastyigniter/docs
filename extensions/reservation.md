---
title: "Reservation Extension"
section: "extensions"
sortOrder: 90
---

## Introduction

This extension to the TastyIgniter built-in reservation management provides a simple booking form to accept table reservations.

## Installation

To install this extension, click on the **Add to Site** button on the TastyIgniter marketplace item page or search for **Igniter.Reservation** in **Admin System > Updates > Browse Extensions**

## Admin Panel
In the admin user interface you can:
- Define time interval (time slot)
- Define stay time (reservation length)
- Manage your tables (units & capacity)

Go to **Restaurants > Locations > Edit Location**, under the **Orders & Reservations** tab

## Components
| Name     | Page variable                | Description                                      |
| -------- | ---------------------------- | ------------------------------------------------ |
| Booking | `@component('booking')` | Display the booking form              |
| Reservations | `@component('reservations')` | Displays a list of reservations on the page               |

### Booking Component

**Properties**

| Property                 | Description              | Example Value | Default Value |
| ------------------------ | ------------------------ | ------------- | ------------- |
| useCalender      | Use the calender view     |       TRUE/FALSE           |        TRUE   |
| minGuestSize      | The minimum guest size        |       20           |      20   |
| maxGuestSize      | The maximum guest size        |       20           |      20   |
| timeSlotsInterval     | The interval to use for the time slots        |       15           |      15   |
| showLocationThumb     | Show Location Image Thumbnail     |       FALSE           |      FALSE   |
| locationThumbWidth        | Location thumb Height        |        95           |      95    |
| locationThumbHeight       | Location thumb Width     |        80           |      80    |
| bookingPage       | Booking page name      |      reservation/reservation           |     reservation/reservation  |
| successPage       | Page name to redirect to when checkout is successful       |      reservation/success           |     reservation/success  |

**Variables available in templates**

| Variable                  | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `{{ $bookingDateFormat }}` | Date format                                                |
| `{{ $bookingTimeFormat }}` | Time format                                               |
| `{{ $bookingDateTimeFormat }}` | Date time format                                                |
| `{{ $bookingLocation }}` | Instance of the current location model                                              |
| `{{ $showLocationThumb }}` | Display location thumbnail                                                |
| `{{ $locationThumbWidth }}` | Location thumbnail width                                                |
| `{{ $locationThumbHeight }}` | Location thumbnail height                                               |
| `{{ $customer }}` | Instance of the logged user model                                                |

**Example:**

```
---
title: 'Reservation'
permalink: /reservation

'[booking]': { }
---
...
@component('booking')
...
```

## Automations Events
- New Reservation Event
- Reservation Status Update Event
- Reservation Assigned Event

## Automations Conditions
- Reservation Attributes
- Reservation Status Attributes

## Notifications

- Reservation confirmation notification
- Reservation status update notification
- Reservation assigned notification

## Events

The Booking Manager used with this extension will fire some global events that can be useful for interacting with other extensions.

| Event | Description | Parameters |
| ----- | ----------- | ---------- |
| `igniter.reservation.beforeSaveReservation` |    When a reservation is being created.    |           |
| `igniter.reservation.confirmed` |      When a reservation has been placed.       |      The `CartItem` instance     |

**Example of hooking an event**

```
Event::listen('igniter.reservation.confirmed', function($cartItem) {
    // ...
});
```

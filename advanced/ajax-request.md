---
title: "Handling AJAX Requests"
section: advanced
sortOrder: 300
---

## Introduction

TastyIgniter provides a simple and easy way to make AJAX requests to the server using the `$.request` JavaScript object. This object is a wrapper around the jQuery AJAX function, providing a more convenient way to make AJAX requests.

In this guide, you'll learn how to make AJAX requests using the `$.request` object, handle AJAX requests on the server, and customize AJAX requests using options.

Alternatively, you can use [Livewire's Event Listeners](https://livewire.laravel.com/docs/actions#event-listeners) to make AJAX requests. 

## How AJAX requests work

The `$.request` object is a wrapper around the jQuery AJAX function, providing a more convenient way to make AJAX requests. It is used to send an HTTP request to the server and receive a response from the server without reloading the page.

Here's an example of how to make an AJAX request using the `$.request` object:

```javascript
$.request('onSave', {
    data: {
        var1: 'some string',
        var2: 'another string'
    },
    success: function(data) {
        console.log(data);
    }
});
```

In this example, the `$.request` object is used to send an AJAX request to the server. The first argument is the name of the handler method on the server that will handle the request. The second argument is an object containing the request data and a callback function to handle the response.

You can use data attributes to trigger AJAX requests by adding the `data-request` attribute to HTML elements. The value of the `data-request` attribute should be the name of the handler method on the server that will handle the request.

```blade
<button data-request="onSave" data-request-data="'var1': 'some string', 'var2': 'another string'">Save</button>
```

## Using AJAX handlers

To handle an AJAX request on the server, you need to create a handler method in your controller that will process the request and return a response. The handler method should be named `on{HandlerName}`.

Here's an example of a handler method:

```php
public function onSave()
{
    $param1 = post('param1');
    $param2 = post('param2');

    // Process the request data

    return ['result' => 'success'];
}
```

You can pass data to the server using the `data` option in the `$.request` object. The data should be an object containing key-value pairs of the request data.

Here's an example of how to pass data to the server in an AJAX request:

```javascript
$.request('onSave', {
    data: {
        param1: 'value1',
        param2: 'value2'
    },
    success: function(data) {
        console.log(data);
    }
});
```

In this example, the `data` option is used to pass data to the server in the AJAX request.

## Handling the response

You can handle the response from the server using the `success` option in the `$.request` object. The `success` option should be a callback function that will be called when the server returns a response. Additionally, you can use the `done` promise method to handle errors.

Here's an example of how to handle the response from the server in an AJAX request:

```javascript
$.request('onSave', {
    success: function(data) {
        console.log(data);
    }
});
```

And here's how you can handle the response using the `done` promise method:

```javascript
$.request('onSave').done(function(data) {
    console.log(data);
});
```

## Error handling

You can handle errors that occur during the AJAX request using the `error` option in the `$.request` object. The `error` option should be a callback function that will be called when an error occurs during the request. Additionally, you can use the `fail` promise method to handle errors.

Here's an example of how to handle errors in an AJAX request:

```javascript
$.request('onSave', {
    error: function(xhr, status, error) {
        console.log('An error occurred: ' + error);
    }
});
```

And here's how you can handle errors using the `fail` promise method:

```javascript
$.request('onSave').fail(function(xhr, status, error) {
    console.log('An error occurred: ' + error);
});
```

## Request options

The `$.request` object supports additional options for customizing AJAX requests. These options can be defined either programmatically using JavaScript or through the data attributes API by adding data attributes to HTML elements.

Some of the common options include:

### `update`

_(Object)_ Specifies a list of partials and page elements to be updated with the response data. The key is the partial name, and the value is the CSS selector of the target element to be updated.

**JavaScript:**
```javascript
$.request('onSave', {
    update: {
        'partial1': '#element1',
        'partial2': '#element2'
    }
});
```
**Data attribute**
```html
<button data-request="onSave" data-request-update="'partial1': '#element1', 'partial2': '#element2'">Save</button>
```

> You may prepend the CSS Selector with `@` to append contents to the element, `^` to prepend and `~` to replace with.

### `confirm`

_(string)_ Specifies a confirmation message that will be displayed to the user before sending the request. If the user confirms the request, the request will be sent; otherwise, the request will be canceled.

**JavaScript:**
```javascript
$.request('onSave', {
    confirm: 'Are you sure?'
});
```
**Data attribute:**
```html
<button data-request="onSave" data-request-confirm="Are you sure?">Delete</button>
```

### `data`

_(Object)_ Specifies the data to be sent with the request. The data should be an object containing key-value pairs of the request data.

**JavaScript:**
```javascript
$.request('onSave', {
    data: {
        param1: 'value1',
        param2: 'value2'
    }
});
```
**Data attribute:**
```html
<button data-request="onSave" data-request-data="'var1': 'some string', 'var2': 'another string'">Save</button>
```

### `redirect`

_(string)_ Specifies the URL to redirect to after the request is completed.

**JavaScript:**
```javascript
$.request('onSave', {
    redirect: '/success'
});
```
**Data attribute:**
```html
<button data-request="onSave" data-request-redirect="/success">Save</button>
```

### `headers`

_(Object)_ Specifies additional headers to be sent with the request.

**JavaScript:**
```javascript
$.request('onSave', {
    headers: {
        'X-CSRF-TOKEN': 'token'
    }
});
```

### `attach-loading`

_(string)_ Specifies the CSS selector to add to the target element while the request is loading. The attribute value is optional, if not provided, the target element will be disabled.

**Data attribute:**
```html
<button data-request="onSave" data-attach-loading="disabled">Save</button>
```

### `replace-loading`

_(string)_ Specifies the CSS selector to replace on the target element while the request is loading.

**Data attribute:**
```html
<button data-request="onSave" data-replace-loading="fa-spin fa-spinner">Save</button>
```

### `beforeUpdate`

_(function)_ Specifies a callback function or Javascript code to be executed before updating the target element with the response.

**JavaScript:**
```javascript
$.request('onSave', {
    beforeUpdate: function(data) {
        console.log('Before update');
    }
});
```
**Data attribute**
```html
<button data-request="onSave" data-request-before-update="console.log('Before update')">Save</button>
```

### `success`

_(function)_ Specifies a callback function or Javascript code to be executed after the request is successful.

**JavaScript:**
```javascript
$.request('onSave', {
    success: function(data) {
        console.log('Success');
    }
});
```
**Data attribute**
```html
<button data-request="onSave" data-request-success="console.log('Success')">Save</button>
```

### `error`

_(function)_ Specifies a callback function or Javascript code to be executed when an error occurs during the request.

**JavaScript:**
```javascript
$.request('onSave', {
    error: function(xhr, status, error) {
        console.log('An error occurred: ' + error);
    }
});
```
**Data attribute**
```html
<button data-request="onSave" data-request-error="console.log('An error occurred')">Save</button>
```

### `complete`

_(function)_ Specifies a callback function or Javascript code to be executed after the request is completed.

**JavaScript:**
```javascript
$.request('onSave', {
    complete: function(xhr, status) {
        console.log('Request completed');
    }
});
```
**Data attribute**
```html
<button data-request="onSave" data-request-complete="console.log('Request completed')">Save</button>
```

### `submit`

When set to `true`, the form will be submitted using the default form submission method.

**JavaScript:**
```javascript
$.request('onSave', {
    submit: true
});
```
**Data attribute**
```html
<button data-request="onSave" data-request-submit="true">Save</button>
```

### `form`

_(CSS Selector)_ Specifies the form element to be submitted. Useful when the request is triggered by a button outside the form.

**JavaScript:**
```javascript
$.request('onSave', {
    form: '#myForm'
});
```
**Data attribute**
```html
<button data-request="onSave" data-request-form="#myForm">Save</button>
```

## Global AJAX events

The AJAX framework triggers several events on the updated elements, the triggering element, form, and the window object. These events are triggered regardless of which API was used - the data attributes API or the JavaScript API.

### `ajaxBeforeSend`

Triggered on the window object before the AJAX request is sent. The handler receives the context object as an argument.

```javascript
$(window).on('ajaxBeforeSend', function(context) {
    console.log(context);
});
```

### `ajaxBeforeUpdate`

Triggered on the form element immediately after the request is completed but before updating the page. The event handler receives the `event` object, the `context` object, the `data` object received from the server, the `status` text string, and the `jqXHR` object as arguments.

```javascript
$('form').on('ajaxBeforeUpdate', function(event, context, data, status, jqXHR) {
    console.log(event, context, data, status, jqXHR);
});
```

### `ajaxUpdate`

Triggered on the page element after updating the element with the response data. The event handler receives the `event` object, the `context` object, the `data` object received from the server, the `status` text string, and the `jqXHR` object as arguments.

```javascript
$('#element').on('ajaxUpdate', function(event, context, data, status, jqXHR) {
    console.log(event, context, data, status, jqXHR);
});
```

### `ajaxUpdateComplete`

Triggered on the window object after all element are updated. The event handler receives the `event` object, the `context` object, the `data` object received from the server, the `status` text string, and the `jqXHR` object as arguments.

```javascript
$(window).on('ajaxUpdateComplete', function(event, context, data, status, jqXHR) {
    console.log(event, context, data, status, jqXHR);
});
```

### `ajaxSuccess`

Triggered on the form element after the request is successful. The event handler receives the `event` object, the `context` object, the `data` object received from the server, the `status` text string, and the `jqXHR` object as arguments.

```javascript
$('form').on('ajaxSuccess', function(event, context, data, status, jqXHR) {
    console.log(event, context, data, status, jqXHR);
});
```

### `ajaxError`

Triggered on the form element when an error occurs during the AJAX request. The event handler receives the `event` object, the `context` object, the `error` message, the `status` text string, and the `jqXHR` object as arguments.

```javascript
$('form').on('ajaxError', function(event, context, error, status, jqXHR) {
    console.log(event, context, data, status, jqXHR);
});
```

### `ajaxErrorMessage`

Triggered on the window object when an error occurs during the AJAX request. The event handler receives the `event` object and `error` message as an argument.

```javascript
$(window).on('ajaxErrorMessage', function(event, error) {
    console.log(event, error);
});
```

### `ajaxConfirmMessage`

Triggered on the window object when a `confirm` option is given. The event handler receives the event object and the confirmation message as arguments.

```javascript
$(window).on('ajaxConfirmMessage', function(event, message) {
    console.log(event, message);
});
```

### `ajaxSetup`

Triggered on the triggering element before the AJAX request is formed. The event handler receives the `context` object as an argument.

```javascript
$('#element').on('ajaxSetup', function(context) {
    console.log(context);
});
```

### `ajaxPromise`

Triggered on the triggering element before the AJAX request is sent. The event handler receives the `context` object as an argument.

```javascript
$('#element').on('ajaxPromise', function(context) {
    console.log(context);
});
```

### `ajaxDone`

Triggered on the triggering element when the AJAX request is successful. The event handler receives the `context` object, the `data` object received from the server, the `status` text string, and the `jqXHR` object as arguments.

```javascript
$('#element').on('ajaxDone', function(context, data, textStatus, jqXHR) {
    console.log(data);
});
```

### `ajaxFail`

Triggered on the triggering element when the AJAX request fails. The event handler receives the `context` object, the `status` text string, and the `jqXHR` object as arguments.

```javascript
$('#element').on('ajaxFail', function(context, textStatus, jqXHR) {
    console.log(jqXHR, textStatus, errorThrown);
});
```

### `ajaxAlways`

Triggered on the triggering element when the AJAX request is completed. The event handler receives the `context` object, the `data` object received from the server, the `status` text string, and the `jqXHR` object as arguments.

```javascript
$('#element').on('ajaxAlways', function(context, dataOrXhr, textStatus, xhrOrError) {
    console.log(jqXHR, textStatus);
});
```

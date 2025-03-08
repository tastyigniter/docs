---
title: "Mail"
section: advanced
sortOrder: 320
---

## Introduction

TastyIgniter uses the Laravel Mail component to send emails. The Mail component provides a clean, simple API which allows you to send emails through a variety of drivers, including SMTP, Mailgun, Postmark and Amazon SES.

### Driver prerequisites

To use the Mailgun, Postmark and Amazon SES drivers, install the required dependencies via Composer.

#### Mailgun

```bash
composer require symfony/mailgun-mailer symfony/http-client
```

#### Postmark

```bash
composer require symfony/postmark-mailer symfony/http-client
```

#### Amazon SES

```bash
composer require aws/aws-sdk-php
```

## Configuration

Next, you can configure your mail settings from the _Manage > Settings > Mail_ admin settings page or through the mail configuration file located at `config/mail.php`. In this file, you may configure the default mail driver, mail sending options, and mail "from" address. Next, verify that your `config/services.php` configuration file contains the required credentials for your mail service.

## Writing mail

In TastyIgniter, you can send mail messages using either mail templates or mail views. Mail templates can be managed through the _Design > Mail templates_ admin page.  On the other hand, mail views supplied by the application or extension are stored in the `resources/views` directory within the extension's directory.

Optionally, you can [register mail views in the Extension class](../advanced/mail#registering-mail-templates-layouts--partials) with the `registerMailTemplates` method. This enables automatic generation of mail templates for easy customization via the admin interface.

> All mail messages support using Blade and Markdown syntax for markup.

### Creating mail views

Mail views are stored in the file system, and the _mail code_ is used to represent the path to the view file. For example, sending mail with the code `vendor.extension::mail.message` would use the content in the corresponding file at `vendor/extension/views/mail/message.blade.php`.

The mail view file content can include up to 3 sections: **configuration**, **plain text**, and **HTML markup**. These sections are separated using the `==` sequence. For example:

```blade
subject = "Your order has been placed"
==
Hello {{ $customer->first_name }},

Your order has been placed successfully.

Thank you for your order.
==
<p>Hello {{ $customer->first_name }},</p>

<p>Your order has been placed successfully.</p>

<p>Thank you for your order.</p>
```

The **configuration** section sets the mail view parameters. The following configuration parameters are supported:

| Parameter   | Description                                                   |
|-------------|---------------------------------------------------------------|
| `subject`   | the mail message subject, **required**.                       |
| `layout`    | the mail layout code, **optional**. Default value is default. |

The **plain text** section is optional, while the **configuration** and **HTML markup** sections are required.

```blade
subject = "Your order has been placed"
==
<p>Hello {{ $customer->first_name }},</p>

<p>Your order has been placed successfully.</p>

<p>Thank you for your order.</p>
```

### Creating mail templates

Mail templates are used to define the structure of the mail message. You can create mail templates in the admin interface by navigating to _Design > Mail templates_. The `code` specified in the template is a unique identifier and cannot be changed once created.

Here is an example of a simple mail template:

```blade
subject: Your order has been placed
==
<p>Hello {{ $customer->first_name }},</p>

<p>Your order has been placed successfully.</p>

<p>Thank you for your order.</p>
```

### Creating mail layouts

Mail layouts can be created by navigating to _Design > Mail templates > Layouts_ in the admin interface. Mail layouts are used to define the structure of the mail message, including the header, footer, and other common elements. The `code` specified in the layout is a unique identifier and cannot be changed once created.

By default, TastyIgniter comes with a `default` mail layout that can be used as a starting point for creating custom mail layouts.

Here is an example of a simple mail layout:

```blade
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>
    <style type="text/css">
        {{ $custom_css }}
        {{ $layout_css }}
    </style>
    <header>
        <h1>{{ $subject }}</h1>
    </header>

    <main>
        {!! $body !!}
    </main>

    <footer>
        <p>&copy; {{ date('Y') }} TastyIgniter</p>
    </footer>
</body>
</html>
```

In this example, the layout includes a header, main content, and footer sections. The `{{ $custom_css }}` variable is used to include custom CSS styles in the mail layout,  and the `{{ $layout_css }}` variable is used to include the mail layout CSS styles defined in the admin interface. The `{{ $subject }}` and `{!! $body !!}` variables are used to include the mail message subject and content, respectively.

### Creating mail partials

Mail partials are reusable components that can be included in mail templates and layouts. You can create mail partials by navigating to _Design > Mail templates > Partials_ in the admin interface. The `code` specified in the partial is a unique identifier and cannot be changed once created.

Here is an example of a simple mail partial:

```blade
name = "Footer"
==
-------------------
{{ $slot }}
==
<p>{{ $slot }}</p>
```

You can include the mail partial in a mail template or layout using the `@partial` directive:

```blade
@partial('footer')
<p>&copy; {{ date('Y') }} TastyIgniter</p>
@endpartial
```

### Mail variables

You may access all the data passed to the mail view or template by using the `{{ $variable }}` syntax. For example, if you pass a `$customer_name` variable to the mail view, you can access it in the view like this:

```blade
<p>Hello {{ $customer_name }},</p>
```

### Attachments

You can attach files to your emails using the `attach` method on the `Mail` facade. The `attach` method accepts the full path to the file as its first argument:

```php
use Illuminate\Support\Facades\Mail;

Mail::send('vendor.extension::mail.message', $data, function($message) {
    // ...
    $message->attach('/path/to/file');
});
```

#### Inline Attachments

To embed media in your emails, you can use the `embed` method on the `$message` variable:

```html
<body>
    <img src="{{ $message->embed($imagePath) }}">
</body>
```

#### Embedding Raw Data

To embed raw data in your emails, you can use the `embedData` method on the `$message` variable. This method accepts the raw data and the name of the file as its first and second arguments, respectively:

```html
<body>
    <img src="{{ $message->embedData($data, 'name.jpg') }}">
</body>
```

## Sending mail templates

To send a mail template in TastyIgniter, you can use the `sendTemplate` method on the `Mail` facade. This method accepts the code of the mail template, an array of data to pass to the view, and a closure that receives a message instance which allows you to customize the recipients, subject, and other aspects of the mail message:

```php
use Igniter\System\Helpers\MailHelper;

$data = [];

MailHelper::sendTemplate('vendor.extension::mail.message', $data, function($message) use ($customer) {
    $message->to($customer->email, $customer->name);
});
```

### Queueing mail templates

To queue a mail message, you can use the `queueTemplate` method on the `Mail` facade. This method will automatically push the email onto the queue, so it will be sent in the background by a queue worker. This can help to improve the response time of your application by offloading the sending of the email to a background process:

```php
use Igniter\System\Helpers\MailHelper;

$data = [];

MailHelper::queueTemplate('vendor.extension::mail.message', $data, function($message) use ($customer) {
    $message->to($customer->email, $customer->name);
});
```

### The `SendsMailTemplate` model trait

The `SendsMailTemplate` trait provides a convenient way to send mail templates from a model. To use this trait, add it to your model class and define the `mailGetData` method that returns the mail template variables:

```php
use Igniter\Flame\Mail\SendMailTemplate;

class Order extends Model
{
    use SendMailTemplate;

    public function mailGetData()
    {
        return [
            'customer' => $this->customer,
        ];
    }
}
```

You can use the `mailGetRecipients` method to define the recipients of the mail message. The `mailGetRecipients` method accepts a single `$recipientType` parameter and returns an array of recipients, where each recipient is an array containing the email address and name of the recipient.

```php
public function mailGetRecipients($recipientType)
{
    return [
        [$this->customer->email, $this->customer->name],
    ];
}
```

You may also customise the reply-to address by defining the `mailGetReplyTo` method:

```php
public function mailGetReplyTo()
{
    return [$this->customer->email, $this->customer->name];
}
```

To send the mail message, use the `mailSend` method on the model instance, where the first parameter is the mail template code, and the second parameter is the recipient type:

```php
$order = Order::find(1);

$order->mailSend('vendor.extension::mail.message', 'customer');
```

Possible values for the recipient type are `customer`, `location` or `admin`.

## Registering mail templates, layouts & partials

To register mail templates, layouts, and partials in the Extension class, you can use the [`registerMailTemplates`](../extend/extensions#extension-class-methods), [`registerMailLayouts`](../extend/extensions#extension-class-methods), and [`registerMailPartials`](../extend/extensions#extension-class-methods) methods, respectively. These methods allow you to define the mail templates, layouts, and partials that your extension provides, making them available for customization via the admin interface:

```php
public function registerMailTemplates(): array
{
    return [
        'vendor.extension::mail.message' => 'Registered mail template message',
    ];
}

public function registerMailLayouts(): array
{
    return [
        'vendor.extension::mail.layouts.default' => 'Default Layout',
    ];
}

public function registerMailPartials(): array
{
    return [
        'vendor.extension::mail.partials.footer' => 'Footer partial',
    ];
}
```

## Mail local development

When developing locally, you may want to catch all outgoing mail messages and display them in the browser instead of sending them. You can use the `log` mail driver to log all outgoing mail messages to the log file. To enable this driver, set the `MAIL_DRIVER` environment variable to `log` in your `.env` file:

```bash
MAIL_DRIVER=log
```

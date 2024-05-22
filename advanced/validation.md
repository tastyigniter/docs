---
title: "Validation"
section: extend
sortOrder: 370
---

## Introduction

Validation is an essential part of TastyIgniter. It ensures that the data entered by users is accurate and meets the required criteria.

## Defining validation rules

Validation rules are defined as an array of key-value pairs, where the key is the field name, and the value is an array containing one or more validation rules.

```php
$rules = [
    'name' => ['required', 'min:5']
    'email' => ['required', 'email', 'unique:users']
];
```

## Available validation rules

TastyIgniter leverages Laravel's robust validation system, which provides a variety of validation rules that you can use to validate user input. For a comprehensive list of available validation rules and their usage, please refer to the [Laravel Validation Documentation](https://laravel.com/docs/validation#available-validation-rules).

## Conditional validation

You can conditionally apply [validation rules](../advanced/validation#available-validation-rules) based on the value of another field. To do this, you can use the `required_if`, `required_unless`, `required_with`, `required_with_all`, `required_without`, and `required_without_all` rules. For example, to require a field only if another field is present:

```php
$rules = [
    'password' => 'required_with:password_confirmation'
];
```

## Validating arrays

To validate an array of form fields, you can use the `.*` wildcard character to validate each element in the array. For example, to validate an array of email addresses:

```php
$rules = [
    'emails.*' => 'required|email'
];
```

Or, you can validate each element in the array:

```php
$rules = [
    'users.*.email' => 'required|email'
];
```

Or, to validate an array of form fields with a specific index:

```php
$rules = [
    'users.0.email' => 'required|email'
];
```

Or, if the incoming HTTP request contains a `records[name]` field, you may define rules like so:

```php
$rules = [
    'records.name' => ['required', 'min:5']
];
```

## Using the validator

In typical scenarios within TastyIgniter, you should initially capture user input using the `post()` helper function. This input is then passed as the first argument to the `make` method, along with the validation rules as the second argument. Here’s how you can handle this in practice:

```php
$validator = Validator::make($data, [
    'name' => ['required', 'min:5'] // Array
]);
```

To validate multiple fields, simply add each field and its corresponding rules to the validation array.

```php
$validator = Validator::make($data, [
    'name' => 'required',
    'password' => ['required', 'min:8'],
    'email' => ['required', 'email', 'unique:users']
]);
```

### Checking the validation results

Once you've created a Validator instance, you can use the `fails` (or `passes`) method to execute the validation checks.

```php
if ($validator->fails()) {
    // The given data did not pass validation
}
```

If validation has failed, you may retrieve the error messages from the validator.

```php
$messages = $validator->messages();
```

You can also access an array of the failed validation rules, without their accompanying messages, by using the `failed` method.

```php
$failed = $validator->failed();
```

### Throwing validation exceptions

You can also handle validation errors by throwing Laravel's `\Illuminate\Validation\ValidationException`. This exception automatically returns the appropriate HTTP response based on the type of request.

For a traditional HTTP request, it triggers a redirect response to the previous URL, along with the validation errors. If the request is an AJAX request, it instead returns a JSON response that includes the validation errors.

```php
$validator = Validator::make($data, $rules);

if ($validator->fails()) {
    throw new \Illuminate\Validation\ValidationException($validator);
}
```

As a shorter way to validate the form similar to the example above, you can use the `validate` method directly.

```php
$data = Validator::validate($data, $rules);
```

### Customizing the error messages

You can customize the error messages for each field by passing an array of custom messages as the third argument to the `make` method.

```php
$validator = Validator::make($data, $rules, [
    'name.required' => lang('author.extension::default.error_name')
]);
```

### Customizing the validation attributes

You can customize the field names used in the validation error messages by passing an array of custom attributes as the fourth argument to the `make` method.

```php
$validator = Validator::make($data, $rules, $messages, [
    'name' => lang('author.extension::default.label_name')
]);
```

## Using the `Request` facade

Another approach is to use the `Request` facade to validate all user inputs directly. This method simplifies the process by eliminating the need to manually supply the data; you only need to provide the validation rules as the first argument. The `validate` method then returns the filtered user data, including only the attributes and values that were successfully validated.

```php
$data = Request::validate([
    'name' => ['required', 'min:5'],
    'email' => ['required', 'email', 'unique:users']
]);
```

In the example above, the `validate` method will return an array containing the validated data. If the validation fails, an exception will be thrown, and the user will be redirected back to the previous page with the validation errors.

## Using form requests

Form requests provide a convenient way to validate incoming HTTP requests in TastyIgniter. By defining validation rules and error messages in a single location, you can maintain and reuse the validation logic across multiple controller actions. This approach helps to keep your controller actions clean and readable by moving the validation logic out of the controller and into a separate class.

Form Requests are typically stored in the `src/Http/Requests` directory of an extension.

### Creating a form request

Form requests are custom request classes that extends the `Igniter\System\Classes\FormRequest` class, containing both validation and authorization logic. Here is an example of a simple form request class:

```php
namespace Author\Extension\Http\Requests;

class RecordRequest extends \Igniter\System\Classes\FormRequest
{
    public function rules(): array
    {
        return [
            'name' => ['required', 'min:5'],
            'email' => ['required', 'email', 'unique:users']
        ];
    }
}
```

### Applying the form request

To use the form request in your admin controller to validate form fields. You can add the form request class to the controller's `$formConfig` property along with the `FormController` action class.

```php
namespace Author\Extension\Http\Controllers;

class MyController extends \Igniter\Admin\Controllers\Controller
{
    public array $implement = [\Igniter\Admin\Http\Actions\FormController::class];

    public $formConfig = [
        'request' => \Author\Extension\Http\Requests\RecordRequest::class,
        'create' => [
            // ...
        ],
    ];
}
```

### Performing additional validation

After the initial validation, you may need to perform further validation. You can do this by calling the form request's `after` function.

The `after` function should return an array of callables or closures that will be executed after validation is complete. The specified callables will receive an `Illuminate\Validation\Validator` object, allowing you to issue extra error messages as needed:

```php
namespace Author\Extension\Http\Requests;

use Illuminate\Validation\Validator;

class RecordRequest extends \Igniter\System\Classes\FormRequest
{
    public function after(Validator $validator): array
    {
        return [
            new \Author\Extension\Validation\ValidateUserStatus,
            function ($validator) {
                if ($this->somethingElseIsInvalid()) {
                    $validator->errors()->add('field', 'Something is wrong with this field!');
                }
            },
        ];
    }
}
```

### Stopping on the first validation failure

By default, the form request will continue to validate all fields, even if one field fails validation. If you want to stop validation on the first failure, you can set the `stopOnFirstFailure` property to `true` in the form request class.

```php
protected $stopOnFirstFailure = true;
```

### Customizing the redirect location

By default, if validation fails, the user is redirected back to the previous page. However, you can customize this behavior in your form request class. To specify a different redirect location, you can override the `$redirect` property. Alternatively, if you prefer to redirect users to a specific named route, you can set the `$redirectRoute` property like so:

```php
protected $redirect = '/custom-url';  // Redirects to a custom URL

// Or

protected $redirectRoute = 'route-name';  // Redirects to a named route
```

### Customizing the error messages

You can customize the error messages for each field by overriding the `messages` method in the form request class.

```php
public function messages(): array
{
    return [
        'name.required' => lang('author.extension::default.error_name')
    ];
}
```

### Customizing the validation attributes

You can customize the field names used in the validation error messages by overriding the `attributes` method in the form request class.

```php
public function attributes(): array
{
    return [
        'name' => lang('author.extension::default.label_name')
    ];
}
```

---
title: "PHP Coding Guidelines"
section: advanced
sortOrder: 360
---

## PHP Coding guidelines and standards

Consistency is key

Coding Standards are critical for achieving high code quality. We can produce a homogeneous code that is easy to read and maintain by using a consistent visual style, naming conventions, and other technical settings.

## General PHP Rules

Code style must follow [PSR-1](http://www.php-fig.org/psr/psr-1/), [PSR-2](http://www.php-fig.org/psr/psr-2/) and [PSR-12](https://www.php-fig.org/psr/psr-12/). Generally speaking, everything string-like (except Model attributes) should use camelCase. Detailed examples on these are spread throughout the guide in their relevant sections.

#### Class defaults

At TastyIgniter, we don't use `final` by default.

#### Nullable and union types

Whenever possible use the short nullable notation of a type, instead of using a union of the type with `null`.

**Good:**

```php
public ?string $variable;
```

**Bad:**

```php
public string|null $variable;
```

## Naming Convention

Naming things is often seen as one of the harder things in programming. That's why we've established some high level guidelines for naming classes.

Also, follow naming conventions accepted by Laravel community:

| What  |  How  |  Good  |  Bad
|-------|-------|--------|-----------------
**Controller**  |  plural  |  `ArticlesController`  |  `ArticleController`
**Route**  |  plural  |  `articles/1`  |  `article/1`
**Model**  |  singular  |  `User`  |  `Users`
**Table**  |  plural  |  `article_comments`  |  `article_comment, articleComments`
**Pivot table**  |  singular model names  |  `article_user`  |  `articles_users`
**Table column**  |  snake_case without model name  |  `meta_title`  |  `MetaTitle, article_meta_title`
**Foreign key**  |  singular model name with _id suffix  |  `article_id`  |  `ArticleId, id_article, articles_id`
**Primary key**  |  -  |  `id`  |  `custom_id`
**Migration**  |  -  |  `2017_01_01_000000_create_articles_table`  |  `2017_01_01_000000_articles`
**Method**  |  camelCase  |  `getAll`  |  `get_all`
**Function**  |  snake_case  |  `abort_if`  |  `abortIf`
**Method in test class**  |  camelCase  |  `testGuestCannotSeeArticle`  |  `test_guest_cannot_see_article`
**Model property**  |  snake_case  |  `$model->model_property`  |  `$model->modelProperty`
**Variable**  |  camelCase  |  `$anyOtherVariable`  |  `$any_other_variable`
**Collection**  |  descriptive, plural  |  `$activeUsers = User::active()->get()`  |  `$active, $data`
**Object**  |  descriptive, singular  |  `$activeUser = User::active()->first()`  |  `$users, $obj`
**Config and language files index**  |  snake_case  |  `articles_enabled`  |  `ArticlesEnabled, articles-enabled`
**View file name**  |  kebab-case  |  `show-filtered.blade.php`  |  `showFiltered.blade.php, show_filtered.blade.php`
**Config file name**  |  kebab-case  |  `google-calendar.php`  |  `googleCalendar.php, google_calendar.php`
**Contract (interface)**  |  adjective or noun  |  `Authenticatable`  |  `AuthenticationInterface, IAuthentication`
**Trait**  |  adjective  |  `Notifiable`  |  `NotificationTrait`

### Jobs

A job's name should describe its action and an optional `Job` suffix..

Example: `CreateUser` or `PerformDatabaseCleanupJob`

### Events

Events will often be fired before or after the actual event. This should be very clear by the tense used in their name.

Example: `ApprovingCustomer` before the action is completed and `CustomerApproved` after the action is completed.

### Listeners

Listeners will perform an action based on an incoming event. Their name should reflect that action.

Example: `SendInvitationMailListener`

## Shorter and readable syntax

Use shorter and more readable syntax where possible.

**Good:**

```php
session('cart');
$request->name;
```

**Bad:**

```php
$request->session()->get('cart');
$request->input('name');
```

### More examples:

Consider using helpers instead of facades. They can clean up your code.

Common syntax  |  Shorter and more readable syntax
---------------|------------------------------------
`Session::get('foo')`  |  `session('foo')`
`$request->session()->get('foo')`  |  `session('foo')`
`Session::put('foo', $data)`  |  `session(['foo' => $data])`
`$request->input('name'),Request::get('name')`  |  `$request->name,request('name')`
`return Redirect::back()`  |  `return redirect()->back()`
`is_null($object->relation) ? $object->relation->id : null;`  |  `optional($object->relation)->id`
`return view('index')->with('title', $title)->with('client', $client)`  |  `return view('index', compact('title', 'client'))`
`$request->has('value') ? $request->value : 'default';`  |  `$request->get('value','default')`
`Carbon::now(), Carbon::today()`  |  `now(), today()`
`App::make('Class')`  |  `app('Class')`
`->where('column', '=', 1)`  |  `->where('column', 1)`
`->orderBy('created_at', 'desc')`  |  `->latest()`
`->orderBy('age', 'desc')`  |  `->latest('age')`
`->orderBy('created_at', 'asc')`  |  `->oldest()`
`->select('id', 'name')->get()`  |  `->get(['id', 'name'])`
`->first()->name`  |  `->value('name')`

## Docblocks

Don't use docblocks for methods that can be fully type hinted (unless you need a description).

Only add a description when it provides more context than the method signature itself. Use full sentences for descriptions, including a period at the end.

**Good:**

```php
class Foo
{
    public static function fromString(string $bar): Foo
    {
        // ...
    }
}
```

**Bad:**

```php
// Bad: The description is redundant, and the method is fully type-hinted.
class Foo
{
    /**
     * Create a url from a string.
     *
     * @param string $bar
     *
     * @return \Igniter\Flame\Foo
     */
    public static function fromString(string $bar): Foo
    {
        // ...
    }
}
```

Always use fully qualified class names in docblocks.

**Good:**

```php
/**
 * @param string $url
 *
 * @return \Igniter\Flame\Foo
 */
```

**Bad:**

```php
/**
 * @param string $foo
 *
 * @return Foo
 */
```

Using multiple lines for a docblock, might draw too much attention to it. When possible, docblocks should be written on one line.

**Good:**

```php
/** @var string */
/** @test */
```

**Bad:**

```php
/** * @test */
```

If a variable has multiple types, the most common occurring type should be first.

**Good:**

```php
/** @var \Igniter\Flame\Foo|null */
```

**Bad:**

```php
/** @var null|\Igniter\Flame\Foo */
```

## Comments

Comments should be avoided as much as possible by writing expressive code. If you do need to use a comment, format it like this:

```php
// There should be a space before a single line comment.

/*
 * If you need to explain a lot you can use a comment block. Notice the
 * single * on the first line. Comment blocks don't need to be three
 * lines long or three characters shorter than the previous line.
 */
```

A possible strategy to refactor away a comment is to create a function with name that describes the comment

**Good:**

```php
$this->calculateTotals();
```

**Bad:**

```php
// Start calculating totals
```

Comment your code, but prefer descriptive method and variable names over comments

**Good:**

```php
if ($this->hasJoins())
```

**Bad:**

```php
if (count((array) $builder->getQuery()->joins) > 0)
```

## Whitespace

Statements should be allowed to breathe. In general always add blank lines between statements, unless they're a sequence of single-line equivalent operations. This isn't something enforceable, it's a matter of what looks best in its context.

**Good:**

```php
public function getPage($url)
{
    $page = $this->pages()->where('slug', $url)->first();

    if (!$page)
        return null;

    if ($page['private'] && !Auth::check())
        return null;

    return $page;
}
```

**Bad:**

```php
// Everything's cramped together.
public function getPage($url)
{
    $page = $this->pages()->where('slug', $url)->first();
    if (!$page)
        return null;
    if ($page['private'] && !Auth::check())
        return null;
    return $page;
}
```

**Good:**

```php
// A sequence of single-line equivalent operations.
public function up()
{
    Schema::create('users', function (Blueprint $table) {
        $table->increments('id');
        $table->string('name');
        $table->string('email')->unique();
        $table->string('password');
        $table->rememberToken();
        $table->timestamps();
    });
}
```

Don't add any extra empty lines between `{}` brackets.

**Good:**

```php
if ($foo) {
    $this->foo = $foo;
}
```

**Bad:**

```php
if ($foo) {

    $this->foo = $foo;

}
```

## Single responsibility principle

A class and a method should have only one responsibility.

**Good:**

```php
public function getFullNameAttribute()
{
    return $this->isVerifiedClient() ? $this->getFullNameLong() : $this->getFullNameShort();
}

public function isVerifiedClient()
{
    return auth()->user() && auth()->user()->hasRole('client') && auth()->user()->isVerified();
}

public function getFullNameLong()
{
    return 'Mr. ' . $this->first_name . ' ' . $this->middle_name . ' ' . $this->last_name;
}

public function getFullNameShort()
{
    return $this->first_name[0] . '. ' . $this->last_name;
}
```

**Bad:**

```php
public function getFullNameAttribute()
{
    if (auth()->user() && auth()->user()->hasRole('client') && auth()->user()->isVerified()) {
        return 'Mr. ' . $this->first_name . ' ' . $this->middle_name . ' ' . $this->last_name;
    } else {
        return $this->first_name[0] . '. ' . $this->last_name;
    }
}
```

## Fat models, skinny controllers

Put all DB related logic into Eloquent models if you’re using Query Builder or raw SQL queries.

**Good:**

```php
public function index()
{
    return view('index', ['clients' => $this->client->getWithNewOrders()]);
}

class Client extends Model
{
    public function getWithNewOrders()
    {
        return $this->verified()
            ->with(['orders' => function ($q) {
                $q->where('created_at', '>', Carbon::today()->subWeek());
            }])
            ->get();
    }
}
```

**Bad:**

```php
public function index()
{
    $clients = Client::verified()
        ->with(['orders' => function ($q) {
            $q->where('created_at', '>', Carbon::today()->subWeek());
        }])
        ->get();

    return view('index', ['clients' => $clients]);
}
```

## Facades

For facades, always use the fully qualified class name

**Good:**

```php
use Illuminate\Support\Facades\Config;
```

**Bad:**

```php
use Config;
```

## Traits

Each applied trait should go on its own line, and the `use` keyword should be used for each of them. This will result in clean diffs when traits are added or removed.

**Good:**

```php
class MyClass
{
    use TraitA;
    use TraitB;
}
```

**Bad:**

```php
class MyClass
{
    use TraitA, TraitB;
}
```

## Strings

When possible prefer string interpolation above `sprintf` and the `.` operator.

**Good:**

```php
$greeting = "Hi, I am {$name}.";
```

**Bad:**

```php
$greeting = 'Hi, I am ' . $name . '.';
```

## If statements

### Bracket position

Always use curly brackets, except its a one line if condition

**Good:**

```php
if ($condition) {
   $this->doSomething();
   $this->doSomethingElse();
}

if ($conidition)
   $this->doSomething();
```

**Bad:**

```php
if ($condition) 
   $this->doSomething()
   $this->doSomethingElse();
```

### Happy path

Generally a function should have its unhappy path first and its happy path last. In most cases this will cause the happy path being in an unindented part of the function which makes it more readable.

**Good:**

```php
if (! $goodCondition) {
  throw new Exception;
}

// do work
```

**Bad:**

```php
if ($goodCondition) {
 // do work
}

throw new Exception;
```

### Avoid else

In general, `else` should be avoided because it makes code less readable. In most cases it can be refactored using early returns. This will also cause the happy path to go last, which is desirable.

**Good:**

```php
if (!$conditionA) {
   // condition A failed
   return;
}

if (!$conditionB) {
   // condition A passed, B failed
   return;
}

// condition A and B passed
```

**Bad:**

```php
if ($conditionA) {
   if ($conditionB) {
      // condition A and B passed
   }
   else {
     // condition A passed, B failed
   }
}
else {
   // condition A failed
}
```

Another option to refactor an `else` away is using a ternary

**Good:**

```php
$condition
    ? $this->doSomething()
    : $this->doSomethingElse();
```

**Bad:**

```php
if ($condition) {
    $this->doSomething();
}
else {
    $this->doSomethingElse();
}
```

### Compound ifs

In general, separate `if` statements should be preferred over a compound condition. This makes debugging code easier.

**Good:**

```php
if ($conditionA)
   return;

if (!$conditionB)
   return;

if (!$conditionC)
   return;

// do stuff
```

**Bad:**

```php
if ($conditionA && $conditionB && $conditionC) {
  // do stuff
}
```

## Configuration

Configuration files must use kebab-case.

```html
config/
  pdf-generator.php
```

Configuration keys must use snake_case.

```php
// config/pdf-generator.php
return [
    'chrome_path' => env('CHROME_PATH'),
];
```

Avoid using the `env` helper outside of configuration files. Create a configuration value from the `env` variable like above.

**Good:**

```php
// config/api.php
'key' => env('API_KEY'),

// Use the data
$apiKey = config('api.key');
```

**Bad:**

```php
$apiKey = env('API_KEY');
```

## Language strings

Use language strings instead of text in the code

**Good:**

```php
return redirect()->back()->with('message', lang('app.menu_added'));
```

**Bad:**

```php
return redirect()->back()->with('message', 'Your menu has been added!');
```

## Artisan commands

The names given to artisan commands should all be kebab-cased.

**Good:**

```bash  
php artisan delete-old-records
```

**Bad:**

```bash
php artisan deleteOldRecords
```

A command should always give some feedback on what the result is. Minimally you should let the `handle` method spit out a comment at the end indicating that all went well.

```php
// in a Command
public function handle()
{
    // do some work

    $this->comment('All ok!');
}
```

When the main function of a result is processing items, consider adding output inside of the loop, so progress can be tracked. Put the output before the actual process. If something goes wrong, this makes it easy to know which item caused the error.

At the end of the command, provide a summary on how much processing was done.

```php
// in a Command
public function handle()
{
    $this->comment("Start processing items...");

    // do some work
    $items->each(function(Item $item) {
        $this->info("Processing item id `{$item-id}`...");
        $this->processItem($item);
    });

    $this->comment("Processed {$item->count()} items.");
}
```

## Blade Templates

Indent using four spaces.

```
<a href="/open-source">
    Open Source
</a>
```

Don't add spaces after control structures.

```
@if($condition)
    Something
@endif
```

Minimize usage of vanilla PHP in Blade templates.

## Validation

When using multiple rules for one field in a form request, avoid using |, always use array notation. Using an array notation will make it easier to apply custom rule classes to a field.

**Good:**

```php
public function rules()
{
    return [
        'body' => 'required',
        'publish_at' => ['nullable', 'date'],
    ];
}
```

**Bad:**

```php
public function rules()
{
    return [
        'body' => 'required',
        'publish_at' => 'nullable|date',
    ];
}
```

Move validation from controllers to FormRequest classes.

**Good:**

```php
class Post extends FormRequest
{
    public function rules()
    {
        return [
            'body' => 'required',
            'publish_at' => ['nullable', 'date'],
        ];
    }
}
```

**Bad:**

```php
$request->validate([
    'body' => 'required',
    'publish_at' => ['nullable', 'date'],
]);
```

## Eloquent over raw SQL queries

Eloquent is preferred over Query Builder and raw SQL queries. Eloquent allows you to write code that is both readable and maintainable. Eloquent has great built-in tools like soft deletes, events, scopes etc.

**Good:**

```php
Article::has('user.profile')->verified()->latest()->get();
```

**Bad:**

```php
SELECT *
    FROM `articles`WHERE EXISTS (SELECT *
                  FROM `users`WHERE `articles`.`user_id` = `users`.`id`AND EXISTS (SELECT *
                              FROM `profiles`WHERE `profiles`.`user_id` = `users`.`id`)
                  AND `users`.`deleted_at` IS NULL)
    AND `verified` = '1'AND `active` = '1'ORDER BY `created_at` DESC
```

## Collections over arrays

When collections can help you clean up your code, use them.

**Good:**

```php
$collection = collect([
    ['name' => 'Regena', 'age' => null],
    ['name' => 'Linda', 'age' => 14],
    ['name' => 'Diego', 'age' => 23],
    ['name' => 'Linda', 'age' => 84],
]);

$collection->firstWhere('name', 'Linda');
```

**Bad:**

```php
$array = [
    ['name' => 'Regena', 'age' => null],
    ['name' => 'Linda', 'age' => 14],
    ['name' => 'Diego', 'age' => 23],
    ['name' => 'Linda', 'age' => 84],
];

$person = null;
foreach ($array as $value) {
    if ($value['name'] === 'Linda') {
        $person = $value;
    }
}
```

## Eager loading

Eager loading helps in solving the N+1 query problem.

**Good:**

```php
// for 100 users, 2 DB queries will be executed
$users = User::with('profile')->get();
```

**Bad:**

```php
// for 100 users, 101 DB queries will be executed
$users = User::all()
```

The above are sections taken from the [Spatie PHP Code Guidelines](https://spatie.be/guidelines/laravel-php).
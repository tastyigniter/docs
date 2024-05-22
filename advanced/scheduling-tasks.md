---
title: "Scheduling Automated Tasks"
section: advanced
sortOrder: 330
---

## Introduction

TastyIgniter's task scheduler is a powerful feature that allows you to define your command schedule within the application itself. It is built on top of the Laravel's task scheduling system, providing a fluent and expressive interface for defining your schedule.

When using the scheduler, only a single Cron entry is needed on your server. This eliminates the need for multiple Cron entries and allows you to keep your schedule of tasks in source control. For more detailed information, you can refer to the [Laravel Task Scheduling documentation](https://laravel.com/docs/scheduling).

> **Note**: See the [installation guide](../installation#setting-up-the-task-scheduler) for instructions on how to set up the task scheduler.

Task Scheduling in TastyIgniter is the mechanism used to manage time-based tasks. Many essential features, such as update checks and auto-assignment of orders to staff, rely on the scheduler to function.

## Defining Schedules

To define scheduled tasks in TastyIgniter, you can override the `registerSchedule` method within the `Extension` class. This method takes a single argument `$schedule`, which is used to define commands along with their frequency.

Here's an example of how to schedule a task:

```php
// Inside your extension class

public function registerSchedule($schedule)
{
    $schedule->call(function () {
        // Your task logic here
    })->daily();
}
```

In this example, a closure is scheduled to run at midnight every day. Inside the closure, you can define the logic for your task, such as executing a database query to clear a table.

In addition to scheduling closure calls, you may also schedule [console commands](https://laravel.com/docs/artisan), [queued jobs](https://laravel.com/docs/queues) and operating system commands. For example, to schedule a console command, you can use the `command` method:

```php
$schedule->command('cache:clear')->daily();
```

To schedule a queued job, you can use the `job` method:

```php
$schedule->job(new MyJob)->daily();
```

The `exec` method may be used to issue a command to the operating system:

```php
$schedule->exec('node /home/acme/script.js')->daily();
```

More information on task scheduling can be found on the [Laravel Task Scheduling docs](https://laravel.com/docs/scheduling).

## Writing commands

To create a new command, you may use the `make:command` Artisan command.

```bash
php artisan create:command Vendor.Extension CommandName
```

This command will create a new command class in the `app/Console/Commands` directory. The generated command will include a `signature` and `description`, as well as a `handle` method where you may place your command's logic.

```php
namespace Vendor\Extension\Console;

class CommandName extends \Illuminate\Console\Command
{
    protected string $signature = 'extension:command {argument : Description} {--option : Description}';
    
    protected string $description = 'Command description';
    
    public function handle()
    {
        // Your command logic here
    }
}
```

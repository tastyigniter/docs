---
title: "Scheduling Automated Tasks"
section: advanced
sortOrder: 330
---

## Introduction

For each task you needed to schedule on your server, you may have generated a Cron entry in the past. This can quickly become a headache, because your schedule of tasks is no longer in source control and you must SSH into your server to add Cron entries. 

The command scheduler allows you to define your command schedule within the application itself fluently and expressively. Only one single Cron entry is required on your server when using the scheduler. 

> **Note**: See the [installation guide](../installation) for instructions on how to set up the task scheduler.

Task Scheduling is how scheduling time-based tasks are managed in TastyIgniter. Several core features of TastyIgniter, such as checking for updates, use the scheduler. 

## Defining Schedules

You may define all of your scheduled tasks by overriding the `registerSchedule` method within the [Extension registration class](../extend/extensions#registration). The method takes a single argument for `$schedule` and is used together with their frequency to define commands. 

To get started, let's look at an example of how to schedule a task. In this example, we schedule to call a closure at midnight every day. We will execute a database query within the `Closure` to clear a table: 

```php
class Extension extends \System\Classes\BaseExtension
{
    [...]

    public function registerSchedule($schedule)
    {
        $schedule->call(function () {
            \Db::table('recent_users')->delete();
        })->daily();
    }
}
```

In addition to scheduling `Closure` calls, you may also schedule <a href="https://laravel.com/docs/artisan" targer="_blank">console commands</a> and operating system commands. For example, to schedule a console command, you can use the `command` method: 

```php
$schedule->command('cache:clear')->daily();
```

The `exec` method may be used to issue a command to the operating system:

```php
$schedule->exec('node /home/acme/script.js')->daily();
```

More information on task scheduling can be found on the <a href="https://laravel.com/docs/scheduling" targer="_blank">Laravel Task Scheduling docs</a>.
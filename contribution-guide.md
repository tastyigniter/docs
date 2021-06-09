---
title: "Contribution Guide"
section: "getting-started"
sortOrder: 40
---

Interested in contributing to the development of TastyIgniter? That is fantastic! All contributions are appreciated and
welcome: from opening a bug report to creating a pull request.

Before contributing, please read the [code of conduct](code-of-conduct).

To order to learn a little more about how TastyIgniter operates, we suggest that you read the documentation, if you're
just beginning.

This article is a guide for developers interested in contributing code to TastyIgniter.

## New features

If you have a feature idea, the TastyIgniter forum is the best place to suggest it. Please do not use GitHub issues to
suggest a new feature.

Use GitHub only if you plan to contribute and develop a new feature. If you'd like to discuss your idea first, you can
always join us on <a href="https://tastyigniter.com/discord" target="_blank">Discord</a> before posting it "officially"
anywhere.

## Reporting bugs

> Please don't use the main GitHub for reporting issues with extensions or themes. If you have found a bug in an extension or theme, the best place to report it is with the [author](https://tastyigniter.com/marketplace).

Issues are a quick way to point out a bug. If you find a bug in TastyIgniter then please check a few things first:

1. Search the TastyIgniter forum, ask the community if they have seen the bug or know how to fix it.
2. There is not already an open Issue
3. The issue has already been fixed (look for closed Issues)
4. Is it something really obvious that you can fix yourself?

We work hard to process bugs that are reported, to assist with this please ensure the following details are always
included:

- Bug summary: Make sure your summary reflects what the problem is and where it is.
- Reproduce steps: Clearly mention the steps to reproduce the bug.
- Version number: The TastyIgniter version affected.
- Expected behavior: How TastyIgniter should behave on above mentioned steps.
- Actual behavior: What is the actual result on running above steps i.e. the bug behavior - include any error messages.

Please be very clear on your commit messages and pull request, duplicate or empty commit or pull request messages may be
rejected without reason.

## Which Branch?

> **Note:** This section applies specifically to those sending pull requests to any repositories under the <a href="https://github.com/tastyigniter" target="_blank">TastyIgniter</a> organization.

**All** bug fixes should be sent to the latest stable branch. Bug fixes should **never** be sent to the `master` branch.

**Minor** features that are **fully backwards compatible** with the current TastyIgniter release may be sent to the
latest stable branch.

**Major** new features should always be sent to the `master` branch, which contains the upcoming TastyIgniter release.

If you are planning to send pull requests via GitHub to the TastyIgniter repository, we suggest sending them to
the `develop` branch where all the latest updates and bug fixes take place.

## Development setup

<a href="https://github.com/tastyigniter/TastyIgniter" target="_blank">tastyigniter/TastyIgniter</a> is the core
application for installing
<a href="https://github.com/tastyigniter/flame" target="_blank">tastyigniter/flame</a> using Composer. We suggest
forking them and cloning them into a <a href="https://getcomposer.org/doc/05-repositories.md#path" target="_blank">
Composer path repository</a> to work on these:

```bash
git clone https://github.com/tastyigniter/TastyIgniter.git
cd TastyIgniter

# Set up a Composer path repository for TastyIgniter packages
composer config repositories.0 path "packages/*"
git clone https://github.com/tastyigniter/flame.git packages/flame
git clone https://github.com/tastyigniter/ti-ext-frontend.git packages/frontend # etc
```

Next, make sure Composer accepts unstable releases from your local copies by adjusting the value of `minimum-stability`
in `composer.json` to `dev`.

Finally, run `composer install` to complete the installation from the path repositories.

## Development workflow

Follow these steps:

- **Clone** the <a href="https://github.com/tastyigniter/TastyIgniter" target="_blank">tastyigniter/TastyIgniter</a>
  repository
- **Branch** off the **develop** branch into a new feature branch.
- Run **composer** install
- Make your **changes**
- Run ./vendor/bin/phpunit to **test** your code
- **Commit** your code with a descriptive message.
- Submit in the **pull request** on Github

## Reporting security issues

If you wish to contact us about any security vulnerability in TastyIgniter you may find, please send an e-mail to
support@tastyigniter.com

## Coding style

To keep the TastyIgniter codebase clean and consistent, we follow a number of coding style guidelines. Read source code
when in doubt.

TastyIgniter follows
the [PSR-2](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md) coding and
the [PSR-4](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md) autoloading standard.

We do comply with a range of other style rules. Where possible, we use PHP 7 type hinting and return type declarations,
and PHPDoc for inline documentation. Try to imitate the style used by the rest of the codebase.

Do not worry if the style of your code isn't great! After pull requests are merged, StyleCI will automatically merge any
style fixes into TastyIgniter repositories. This helps us to focus on what matters.

## Writing documentation

You are very welcome to contribute to the TastyIgniter documentation. Please follow these rules if you want to
contribute. Here's how styling perfect TastyIgniter documentation pages is done:

- Try not to use H1 headers.
- A TOC list would be generated automatically for each page with at least one H2 header. The TOC would have links to all
  of the page's H2 headers.
- The introductory text would be displayed below the TOC.
- Try to use only H2 and H3 headers.
- Each H2 and H3 header could have a link defined as `<a name="event-handlers"></a>` or have it generated automatically.
- Avoid short, 1 sentence paragraphs. Combine short paragraphs and try to be a bit more verbose.
- Avoid short paragraphs hanging below code sections. Combine these paragraphs with the text above the code blocks.
- Use the inline code tags for all code-related - variable names, function names, syntax examples, etc.
- Don't hesitate to make cross links to other documentation articles. There is no need to add links to the same article
  in the same paragraph.
- For your reference, see the [pages.md](https://github.com/tastyigniter/docs/blob/master/customize/pages.md)
  or [themes.md](https://github.com/tastyigniter/docs/blob/master/customize/themes.md) files.

## Development tools

Most TastyIgniter contributors develop with <a href="https://www.jetbrains.com/phpstorm/download/" target="_blank">
PHPStorm</a>. However, feel free to use your preferred IDE.

To serve a local TastyIgniter website, <a href="https://laravel.com/docs/master/valet" target="_blank">Laravel
Valet</a> (Mac), <a href="https://www.apachefriends.org/index.html" target="_blank">XAMPP</a> (Windows),
and <a href="https://github.com/ThisIsQasim/TastyIgniter" target="_blank">TastyIgniter Docker</a> (Linux) are popular
choices.

> For more information on contributing, read the guide <a href="https://github.com/tastyigniter/TastyIgniter/blob/master/CONTRIBUTING.md" target="_blank">here</a>
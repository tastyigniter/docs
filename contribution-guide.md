---
title: "Contribution Guide"
section: "getting-started"
sortOrder: 40
---

## Writing documentation

You are very welcome to contribute to the TastyIgniter documentation. Please follow these rules if you want to contribute. Here's how styling perfect TastyIgniter documentation pages is done:

- Try not to use H1 headers. 
- A TOC list would be generated automatically for each page with at least one H2 header. The TOC would have links to all of the page's H2 headers. 
- The introductory text would be displayed below the TOC.
- Try to use only H2 and H3 headers.
- Each H2 and H3 header could have a link defined as `<a name="event-handlers"></a>` or have it generated automatically.
- Avoid short, 1 sentence paragraphs. Combine short paragraphs and try to be a bit more verbose.
- Avoid short paragraphs hanging below code sections. Combine these paragraphs with the text above the code blocks.
- Use the inline code tags for all code-related - variable names, function names, syntax examples, etc.
- Don't hesitate to make cross links to other documentation articles. There is no need to add links to the same article in the same paragraph.
- For your reference, see the pages.md or themes.md files.

## New features

If you have a feature idea, the TastyIgniter forum is the best place to suggest it. Please do not use GitHub issues to suggest a new feature.

Use GitHub only if you plan to contribute and develop a new feature. If you'd like to discuss your idea first, you can always join us on [Slack](http://slack.tastyigniter.com/) before posting it "officially" anywhere. 

## Bugs

> Please don't use the main GitHub for reporting issues with extensions or themes. If you have found a bug in an extension or theme, the best place to report it is with the [author](https://tastyigniter.com/marketplace).

Issues are a quick way to point out a bug. If you find a bug in TastyIgniter then please check a few things first:

1. Search the TastyIgniter forum, ask the community if they have seen the bug or know how to fix it.
2. There is not already an open Issue
3. The issue has already been fixed (look for closed Issues)
4. Is it something really obvious that you can fix yourself?

We work hard to process bugs that are reported, to assist with this please ensure the following details are always included:

- Bug summary: Make sure your summary reflects what the problem is and where it is.
- Reproduce steps: Clearly mention the steps to reproduce the bug.
- Version number: The TastyIgniter version affected.
- Expected behavior: How TastyIgniter should behave on above mentioned steps.
- Actual behavior: What is the actual result on running above steps i.e. the bug behavior - include any error messages.

Please be very clear on your commit messages and pull request, duplicate or empty commit or pull request messages may be rejected without reason.

## Which Branch?

> **Note:** This section applies specifically to those sending pull requests to all repositories under the [tastyigniter](https://github.com/tastyigniter) organization.

**All** bug fixes should be sent to the latest stable branch. Bug fixes should **never** be sent to the `master` branch.

**Minor** features that are **fully backwards compatible** with the current TastyIgniter release may be sent to the latest stable branch.

**Major** new features should always be sent to the `master` branch, which contains the upcoming TastyIgniter release.

## Coding style

TastyIgniter follows PSR-1 and PSR-2. The following coding standards should be observed in addition to the PSR-1 and PSR-2 standards: 

- The opening bracket `{` of a class MUST be on the same line as the name of the class.
- Allman type braces MUST be used for functions and control structures.
- Use tabs to indent and spaces to align.

## Reporting security issues

If you wish to contact us about any security vulnerability in TastyIgniter you may find, please send an e-mail to Samuel Adepoyigi at sam@sampoyigi.com

## Development setup



## Development Workflow

Follow these steps:

- **Clone** the tastyigniter/TastyIgniter repository
- **Branch** off the approprite branch into a new feature branch.
- Run **composer** install
- Make your **changes**
- Run ./vendor/bin/phpunit to **test** your code
- Submit in the **pull request** if all is green

## Development Tools

Most TastyIgniter contributors develop with <a href="https://www.jetbrains.com/phpstorm/download/" target="_blank">PHPStorm</a>. However, feel free to use your preferred IDE.

To serve a local TastyIgniter website, <a href="https://laravel.com/docs/master/valet" target="_blank">Laravel Valet</a> (Mac), <a href="https://www.apachefriends.org/index.html" target="_blank">XAMPP</a> (Windows), and <a href="https://github.com/ThisIsQasim/TastyIgniter" target="_blank">TastyIgniter Docker</a> (Linux) are popular choices.

> For more information on contributing, read the guide <a href="https://github.com/tastyigniter/TastyIgniter/blob/master/CONTRIBUTING.md" target="_blank">here</a>
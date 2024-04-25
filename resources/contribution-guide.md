---
title: "Contribution Guide"
section: "resources"
sortOrder: 410
---

Interested in contributing to the development of TastyIgniter? All contributions are appreciated and welcome: from
opening a bug report to creating a pull request.

Before contributing, please read the [code of conduct](../resources/code-of-conduct).

To order to learn a little more about how TastyIgniter operates, we suggest that you read the documentation, if you're
just beginning.

## New features

If you have a feature idea, the [TastyIgniter forum](https://forum.tastyigniter.com/) is the best place to suggest it. Please do not use GitHub issues to
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

When you are creating a bug report, please include as many details as possible. Fill out the required template, the information it asks for helps us resolve issues faster

Please be very clear on your commit messages and pull request, duplicate or empty commit or pull request messages may be
rejected without reason.

## Which Branch?

> **Note:** This section applies specifically to those sending pull requests to any repositories under the <a href="https://github.com/tastyigniter" target="_blank">TastyIgniter</a> organization.

**All** bug fixes should be sent to the latest stable branch that supports bug fixes (currently `4.x` and `master` for packages). Bug fixes
should **never** be sent to the `develop` branch unless they fix features that exist only in the upcoming release.

**Minor** features that are **fully backward compatible** with the current release may be sent to the latest stable
branch.

**Major** new features or features with breaking changes should always be sent to the `develop` branch, which contains
the upcoming release.

If you are unsure if your feature qualifies as a major or minor, please ask Sam Poyigi in the `#core` channel of
the [TastyIgniter Discord server.](https://tastyigniter.com/discord)

## Compiled Assets

If you are submitting a change that will affect a compiled file, do not commit the compiled files. Due to their large
size, they cannot realistically be reviewed by a maintainer. This could be exploited as a way to inject malicious code
into TastyIgniter. In order to defensively prevent this, all compiled files will be generated and committed by
TastyIgniter maintainers.

## Development setup

<a href="https://github.com/tastyigniter/TastyIgniter" target="_blank">tastyigniter/TastyIgniter</a> is the stand-alone
application for installing
<a href="https://github.com/tastyigniter/core" target="_blank">tastyigniter/core</a> using Composer.

- Make sure Composer accepts unstable releases from your local copies by adjusting the value of `minimum-stability`
in `composer.json` to `dev`.
- Run `composer install` to install composer dependencies.
- Finally, run `composer update "tastyigniter/*" --prefer-source` to clone TastyIgniter packages into the `vendor` directory for development.


## Development workflow

Follow these steps:

- Complete the development setup and make your **changes**
- Run ./vendor/bin/phpunit to **test** your code
- Run ./vendor/bin/pint to **fix** any code style issues
- **Commit** your code with a descriptive message.
- Submit in the **pull request** on GitHub

## Development tools

Most TastyIgniter contributors develop with <a href="https://www.jetbrains.com/phpstorm/download/" target="_blank">
PHPStorm</a>. However, feel free to use your preferred IDE.

To serve a local TastyIgniter website, <a href="https://laravel.com/docs/master/valet" target="_blank">Laravel
Valet</a> (Mac), <a href="https://www.apachefriends.org/index.html" target="_blank">XAMPP</a> (Windows),
and <a href="https://github.com/ThisIsQasim/TastyIgniter" target="_blank">TastyIgniter Docker</a> (Linux) are popular
choices.

## Reporting security issues

If you wish to contact us about any security vulnerability in TastyIgniter you may find, please send an e-mail to
support@tastyigniter.com

## Writing documentation

You are very welcome to contribute to the TastyIgniter documentation. Please follow these rules if you want to
contribute. Here's how styling perfect TastyIgniter documentation pages is done:

- Try not to use H1 headers.
- A TOC list would be generated automatically for each page with at least one H2 header. The TOC would have links to all
  of all the page's H2 headers.
- The introductory text would be displayed below the TOC.
- Try to use only H2 and H3 headers.
- Each H2 and H3 header could have a link defined as `<a name="event-handlers"></a>` or have it generated automatically.
- Avoid short, 1 sentence paragraphs. Combine short paragraphs and try to be a bit more verbose.
- Avoid short paragraphs hanging below code sections. Combine these paragraphs with the text above the code blocks.
- Use the inline code tags for all code-related - variable names, function names, syntax examples, etc.
- Don't hesitate to make cross-links to other documentation articles. There is no need to add links to the same article
  in the same paragraph.
- For your reference, see the [pages](https://github.com/tastyigniter/docs/blob/master/customize/pages.md)
  or [themes](https://github.com/tastyigniter/docs/blob/master/customize/themes.md) files.

> For more information on contributing, read the guide <a href="https://github.com/tastyigniter/TastyIgniter/blob/master/CONTRIBUTING.md" target="_blank">here</a>
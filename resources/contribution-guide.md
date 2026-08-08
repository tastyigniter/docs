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

> Please don't use the main GitHub for reporting issues with third party extensions or themes. If you have found a bug in an extension or theme, the best place to report it is with the [author](https://tastyigniter.com/marketplace).

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

**All** bug fixes should be sent to the latest stable branch that supports bug fixes (currently `4.x` ). Bug fixes should **never** be sent to the `develop` branch unless they fix features that exist only in the upcoming release.

If you are unsure, please ask Sam Poyigi in the `#core` channel of
the <a href="https://tastyigniter.com/discord" target="_blank">TastyIgniter Discord</a>

## Compiled Assets

If you are submitting a change that will affect a compiled file, do not commit the compiled files. Due to their large
size, they cannot realistically be reviewed by a maintainer. This could be exploited as a way to inject malicious code
into TastyIgniter. In order to defensively prevent this, all compiled files will be generated and committed by
TastyIgniter maintainers.

## Development environment setup

<a href="https://github.com/tastyigniter/TastyIgniter" target="_blank">tastyigniter/TastyIgniter</a> is the stand-alone
application for installing
<a href="https://github.com/tastyigniter/core" target="_blank">tastyigniter/core</a> using Composer.

- Make sure Composer accepts unstable releases from your local copies by adjusting the value of `minimum-stability`
in `composer.json` to `dev`.
- Run `composer install` to install composer dependencies.
- Finally, run `composer update "tastyigniter/*" --prefer-source` to clone TastyIgniter packages into the `vendor` directory for development.

## How to contribute

If this is your first contribution, start small: fix a typo, improve docs, or pick up a [good first issue]([https://github.com/tastyigniter/TastyIgniter/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22](https://github.com/tastyigniter/TastyIgniter/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)) (when available). Questions are welcome in the `#core` channel on [Discord]([https://tastyigniter.com/discord](https://tastyigniter.com/discord)).

Follow these steps:

1. **Set up your environment**  
   Complete the [Development environment setup](#development-environment-setup) above.
   Most package code lives under `vendor/tastyigniter/` after
   `composer update "tastyigniter/*" --prefer-source`.
2. **Choose the right repository**  
   Identify which [TastyIgniter](https://github.com/tastyigniter) repository owns the
   change (for example `core`, an extension, or the stand-alone `TastyIgniter` app).
   For third party extensions and themes from the marketplace, contribute to the author’s repository
   instead of the main project.
3. **Fork and clone**  
   Fork the repository on GitHub, then work from your fork inside the development
   environment (typically the matching path under `vendor/tastyigniter/`).
4. **Create a branch from the correct base**  
   See [Which Branch?](#which-branch) — usually the latest stable branch (`4.x`) for bug fixes.
   Create a dedicated branch for your work, for example:
   `git checkout -b fix/short-description`
5. **Make a focused change**  
   Keep the pull request limited to one concern.
6. **Test your changes**  
   From the project root, run:
   ```bash
   composer test
   ```
   Fix any failures before opening a pull request.
7. **Commit and push**
   Use a clear, descriptive commit message that explains why the change is needed. Push the branch to your fork.
8. **Open a pull request**
   Open a PR against the correct upstream branch. In the description, include:
    - what problem you are solving
    - how you tested it
    - linked issue number, if any (Fixes #123)

Maintainers may ask for changes — that is normal; reply on the PR and update your branch as needed.

## Development tools

Most TastyIgniter contributors develop with <a href="https://www.jetbrains.com/phpstorm/download/" target="_blank">
PHPStorm</a>. However, feel free to use your preferred IDE.

To serve a local TastyIgniter website, <a href="https://laravel.com/docs/master/valet" target="_blank">Laravel
Valet</a>, <a href="https://herd.laravel.com" target="_blank">Laravel Herd</a> (For Mac and Windows),
and <a href="https://ddev.com" target="_blank">DDEV Docker</a> (Linux) are popular
choices.

## Reporting security issues

If you wish to contact us about any security vulnerability in TastyIgniter you may find, please send an e-mail to
[support@tastyigniter.com](mailto:support@tastyigniter.com)

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

> For more information on contributing, read the guide [here](https://github.com/tastyigniter/TastyIgniter/blob/master/CONTRIBUTING.md)


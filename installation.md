---
title: "Installation"
section: "getting-started"
sortOrder: 10
---

As of Version 4, TastyIgniter can be installed using the **[setup wizard](#setup-wizard-installation)** (recommended for shared hosting), as a [stand-alone application](#stand-alone-installation), or as a [package](#package-installation) inside an existing Laravel application.

## Setup Wizard Installation

The setup wizard is the **recommended** way to install TastyIgniter on shared hosting. **Composer and SSH are not required to run the wizard** — the installer downloads a **distribution release with bundled dependencies** that already includes `vendor/`. You **will** need Composer later to **install and update extensions**.

### Requirements

- **Apache** (with mod_rewrite enabled) or **Nginx**
- **PHP 8.3+** with extensions: bcmath, ctype, curl, dom, exif, gd, intl, json, mbstring, openssl, pdo_mysql, tokenizer, xml, zip
- **MySQL 8.0+** or **MariaDB 10.6+**
- Write permissions on your installation directory

Composer is **not** required for the initial wizard installation. It **is** required to install or update extensions after TastyIgniter is running.

### Installing with the setup wizard

1. Download the [setup wizard ZIP](https://tastyigniter.com/download/setup-wizard).
2. Create a MySQL database and database user in your hosting control panel (new or existing TastyIgniter v3 database for upgrade).
3. Upload and extract the wizard files to your web root so `setup.php` is accessible in the browser.
4. Set write permissions on the `setup/` directory (typically `755` or `775`).
5. Visit `http://yoursite.com/setup.php` in your browser.
6. Follow the wizard:
   - **Get started** — accept the license and wait for requirements to pass
   - **Database** — enter your MySQL connection details (new database or existing TastyIgniter v3 database to upgrade)
   - **Installing** — the wizard downloads and configures TastyIgniter automatically
   - **Complete** — open the admin panel and complete the system setup
1. **Delete `setup.php` and the `setup/` directory** after confirming the site works.

For installation problems, see [Troubleshooting](#troubleshooting).
### What the wizard does automatically

- Downloads the latest **TastyIgniter distribution release with bundled dependencies** from [GitHub Releases](https://github.com/tastyigniter/TastyIgniter/releases) (`tastyigniter-*.zip`, includes `vendor/`)
- Extracts files into your web root (preserving the wizard files until you remove them)
- Writes `.env` with your database credentials and detected site URL
- Runs `igniter:install` in-process via Laravel's **`Artisan::call()`** — no CLI commands needed. Existing TastyIgniter v3 databases are migrated during this step.
- Creates a root `.htaccess` that redirects requests to `public/` when your docroot cannot be changed

### Extensions and Composer

The wizard installs a complete, ready-to-run copy of TastyIgniter with core dependencies already in `vendor/`. That is enough to complete installation and use the admin panel.

**Installing or updating extensions** (from the marketplace or manually) uses Composer. 

If your shared host does not allow Composer, install extensions on a local or staging environment, then upload the changed `vendor/` directory and any new extension files to production.

### Shared hosting tips

**Document root:** TastyIgniter v4 serves from the `public/` directory. Prefer pointing your domain's document root to `public/` in cPanel, Plesk, or DirectAdmin. If you cannot change the docroot, the wizard creates a root `.htaccess` that forwards requests to `public/index.php`.

**PHP version:** Select PHP **8.3 or higher** in your hosting control panel. Enable all required extensions listed above — especially `intl`, which is commonly disabled by default.

**Permissions:** Ensure `storage/` and `bootstrap/cache/` are writable after installation (`755` or `775`).

**Cron:** Add a cron job to run the scheduler every minute:

```bash
* * * * * php /path/to/artisan schedule:run >> /dev/null 2>&1
```

On shared hosts without SSH, use your control panel's cron interface and provide the full path to `artisan`.

**Queue:** Background jobs need a queue worker in production. See [Setting up the queue daemon](#setting-up-the-queue-daemon).

For installation problems, see [Troubleshooting](#troubleshooting).

---

## Stand-alone Installation

For VPS, Docker, or local development with terminal access.

### Requirements

- **Apache** (with mod_rewrite enabled) or **Nginx**
- **PHP 8.3+** with the following extensions: bcmath, pdo_mysql, ctype, curl, openssl, dom, gd, exif, mbstring, json, tokenizer, zip, xml, intl
- **MySQL 8+** or **PostgreSQL 10.0**
- **Composer 2.0** or higher

### Installing TastyIgniter

Use Composer to create a new project:

```bash
composer create-project tastyigniter/tastyigniter tastyigniter
```

Alternatively, download the **distribution release with bundled dependencies** (`tastyigniter-*.zip`) from [GitHub Releases](https://github.com/tastyigniter/TastyIgniter/releases), extract it.

From here, move on to [Setting up TastyIgniter](#setting-up-tastyigniter).

## Package Installation

### Requirements

These are the requirements to run TastyIgniter as a package in a Laravel application:

- **Laravel 11+**
- **MySQL 8+** or **PostgreSQL 10.0**
- **Composer 2.0** or higher

### Installing TastyIgniter

To install TastyIgniter as a package from your command line, run the following command in your Laravel project directory:

```bash
composer require tastyigniter/core
```

From here, you can move on to the [Setting up TastyIgniter](#setting-up-tastyigniter) step.

## Setting up TastyIgniter

**Setup wizard path:** If you used the wizard, `igniter:install` already ran automatically — skip to [Post-installation steps](#post-installation-steps).

**CLI path:** In the TastyIgniter root directory, run:

```bash
php artisan igniter:install
```

The setup command guides you through database configuration, application URL, and administrator details.

> You can safely run the command multiple times if needed.

**Command-line unattended setup**

Provide values in `.env` and run:

```bash
php artisan igniter:install --no-interaction
```

## Post-installation steps

There are some things you may need to set up after the installation is complete.

### Directory permissions

Once TastyIgniter is installed, grant the web server user write access to required directories:

```bash
sudo chmod -R 755 /path/to/tastyigniter
sudo chown -R www-data:www-data /path/to/tastyigniter
```

> You should never set any folder or file to permission level **777**.

Ensure `storage/` and `bootstrap/cache/` are writable.

### Setting up the task scheduler

Add the following cron entry (via `crontab -e` or your hosting control panel):

```bash
* * * * * php /path/to/artisan schedule:run >> /dev/null 2>&1
```

Replace `/path/to/artisan` with the absolute path to the `artisan` file in your TastyIgniter root directory.

> Task Scheduling is how time-based tasks are managed in TastyIgniter. Several core features, such as assigning orders and checking for updates, use the scheduler.

### Setting up the queue daemon

By default, the queue runs synchronously. Update `QUEUE_CONNECTION` in `.env` for async processing.

For production, run a queue worker (often via Supervisor):

```bash
php /path/to/artisan queue:work
```

See the [Laravel Queue docs](https://laravel.com/docs/queues#running-the-queue-listener) for details.

### Application configuration

**Debug mode**

Debug mode is disabled by default in `config/app.php`. Always set `APP_DEBUG=false` in production.

**CSRF protection**

CSRF protection is enabled by default. Disable via the `ENABLE_CSRF` variable in `.env` if needed (not recommended).

### Apache/Nginx configuration

#### Apache configuration

Ensure the `.htaccess` file in the `public/` directory is uploaded. `mod_rewrite` must be enabled and `AllowOverride All` set:

```apache
<Directory "/path/to/tastyigniter/public">
    AllowOverride All
</Directory>
```

Uncomment `RewriteBase` in `.htaccess` when installing in a subdirectory:

```html
RewriteBase /subdirectory/
```

#### Nginx configuration

Include the bundled `.nginx.conf` from the TastyIgniter root:

```html
include /path/to/tastyigniter/.nginx.conf;
```

Example server block:

```html
server {
    listen 80;

    root /path/to/tastyigniter;
    index index.php;

    server_name mytastysite.com;

    include /path/to/tastyigniter/.nginx.conf;
}
```

## Getting Started

Access the administrator panel at `/admin`. Complete the getting started steps on the dashboard to configure your restaurant, menu, and locations.

## Getting help

For installation problems, start with [Troubleshooting](#troubleshooting). You can also:

- Report bugs on the [GitHub issue tracker](https://github.com/tastyigniter/TastyIgniter/issues)
- Share feedback on the [Community Forums](https://forum.tastyigniter.com)
- [Join us on Discord](https://tastyigniter.com/discord)

## Troubleshooting

Use this section when something goes wrong during or after installation. The setup wizard links here from failed requirement checks and install errors.

### Log files

| Log | Location | When to check |
|-----|----------|---------------|
| Setup wizard | `setup/setup.log` | Requirements, download, extract, `.env`, or `igniter:install` failures |
| Application | `storage/logs/laravel.log` | Errors after installation when visiting `/admin` or the storefront |

Enable debug output in the wizard by appending `?debug` to the setup URL (for example, `setup.php?debug`). Remove it once you have finished troubleshooting.

### Setup wizard — server requirements

The **Get started** step checks your server automatically. Resolve every **blocking** failure before continuing.

#### PHP version

- *Symptom:* "PHP 8.3 or higher" fails.
- *Cause:* PHP version is below 8.3.
- *Fix:* Select PHP **8.3 or higher** in your hosting control panel (cPanel → Select PHP Version, Plesk → PHP Settings, or equivalent). The wizard shows your current version under the requirement.

#### Required PHP extensions

- *Symptom:* "Required PHP extensions" fails.
- *Cause:* One or more extensions is disabled.
- *Fix:* Enable **bcmath**, **ctype**, **dom**, **exif**, **gd**, **intl**, **json**, **mbstring**, **openssl**, **tokenizer**, and **xml** in your PHP configuration. The `intl` extension is commonly disabled by default on shared hosting.

#### PDO MySQL

- *Symptom:* "PDO MySQL extension" fails.
- *Cause:* `pdo` or `pdo_mysql` is not loaded.
- *Fix:* Enable the **PDO** and **pdo_mysql** extensions in your hosting panel.

#### cURL

- *Symptom:* "cURL extension" fails.
- *Cause:* cURL is disabled.
- *Fix:* Enable the **curl** extension. TastyIgniter uses cURL for outbound HTTP requests, and the installer needs it to download the release package.

#### ZipArchive

- *Symptom:* "ZipArchive support" fails.
- *Cause:* The **zip** extension is not installed.
- *Fix:* Enable the **zip** PHP extension. It is required to extract the release archive.

#### GitHub Releases connectivity

- *Symptom:* "GitHub Releases connectivity" fails.
- *Cause:* The server cannot reach `github.com` over HTTPS.
- *Fix:* Confirm outbound HTTPS is allowed by your host. Some firewalls block GitHub; ask your host to allow connections to `github.com`. The installer downloads directly from GitHub Releases (not the GitHub API).

#### Writable installation directory

- *Symptom:* "Writable installation directory" fails.
- *Cause:* The web root or `setup/` directory is not writable by the web server user.
- *Fix:* Set permissions on the installation directory and `setup/` to **755** or **775**. On shared hosting, use your file manager or ask your host to make the directory writable. A previous failed install may leave files in `setup/temp/`; that is normal and does not block this check.

#### Memory limit (recommended)

- *Symptom:* Warning for memory limit — installation can still continue.
- *Cause:* `memory_limit` is below 256M.
- *Fix:* Increase `memory_limit` to **256M** or higher in PHP settings for a smoother install.

#### Max execution time (recommended)

- *Symptom:* Warning for max execution time — installation can still continue.
- *Cause:* `max_execution_time` is below 120 seconds.
- *Fix:* Increase `max_execution_time` to **120** or higher, or ask your host to allow longer web requests during installation.

### Setup wizard — database step

#### Connection failed

- *Symptom:* "Connection failed" when submitting database details.
- *Cause:* Wrong hostname, port, username, password, or database name.
- *Fix:* Verify credentials in your hosting control panel. Hostname is usually `localhost` or `127.0.0.1`. Confirm the database user has access to the database you created.

#### MySQL or MariaDB version

- *Symptom:* Error about MySQL or MariaDB version.
- *Cause:* Database server is older than MySQL 8.0 or MariaDB 10.6.
- *Fix:* Upgrade your database server or create the database on a host that meets the minimum version.

#### Database contains unrelated tables

- *Symptom:* "Database contains tables that do not look like TastyIgniter."
- *Cause:* The database has tables from another application using the same table prefix.
- *Fix:* Use a **new empty database**, an **existing TastyIgniter v3 database** (for upgrade), or change the **table prefix** to one that is not already in use.

> **Upgrading from v3:** You can point the wizard at your existing TastyIgniter v3 database. `igniter:install` migrates the schema during the **Installing** step.

### Setup wizard — installing step

The installer runs six sub-steps: prepare → download → extract → config → migrate → finalize. Check `setup/setup.log` for the step that failed.

#### Download failed

- *Symptom:* "Downloading TastyIgniter" fails or times out.
- *Cause:* Network issue, blocked outbound HTTPS, or the distribution release with bundled dependencies is unavailable.
- *Fix:*
  - Confirm the server can reach `https://github.com/tastyigniter/TastyIgniter/releases/latest`.
  - Check [TastyIgniter Releases](https://github.com/tastyigniter/TastyIgniter/releases) for a `tastyigniter-*.zip` asset on the latest release.
  - Retry the install step. Downloads are **resumable** — a partial file in `setup/temp/tastyigniter-release.zip` is reused automatically.

#### Distribution release with bundled dependencies not found

- *Symptom:* Error that the distribution release was not found for a specific version tag.
- *Cause:* The GitHub release exists but the `tastyigniter-*.zip` distribution file has not been published yet.
- *Fix:* Wait for the release asset to be published, or install a specific version by opening `setup.php?version=x.y.z` (for example, `setup.php?version=4.3.0`).

#### Extract failed

- *Symptom:* "Extracting files" fails.
- *Cause:* Missing ZipArchive extension, insufficient disk space, or the web root is not writable.
- *Fix:* Enable the **zip** extension, free disk space, and ensure the installation directory is writable.

#### Writing configuration failed (`.env.example` not found)

- *Symptom:* "`.env.example` was not found in the release package" during the **config** step.
- *Cause:* Files were not fully extracted, or dotfiles were skipped during copy.
- *Fix:* Retry from the **Installing** step. Ensure `bootstrap/cache/` and `storage/` exist and are writable. If the problem persists, delete partially extracted files in the web root (except `setup.php` and `setup/`) and run the wizard again.

#### `tempnam()` or bootstrap cache error

- *Symptom:* `tempnam(): file created in the system's temporary directory` during **config** or **migrate**.
- *Cause:* `bootstrap/cache/` did not exist or was not writable when Laravel bootstrapped.
- *Fix:* Create `bootstrap/cache/` and `storage/framework/cache/` manually, set permissions to **755** or **775**, then retry. The wizard creates these directories automatically on recent versions.

#### Database setup / migration failed (`igniter:install`)

- *Symptom:* "Setting up database" fails.
- *Cause:* Invalid credentials, incompatible database, PHP timeout, or low memory during migrations.
- *Fix:*
  - Verify database credentials.
  - Use a new database or an existing TastyIgniter v3 database.
  - Increase `max_execution_time` and `memory_limit` in PHP settings.
  - Check `setup/setup.log` and `storage/logs/laravel.log` for the exact migration error.
  - Retry the install — completed download steps are skipped if the ZIP in `setup/temp/` is still valid.

#### Installation failed (generic)

- *Symptom:* "Installation failed. Check setup/setup.log or the troubleshooting guide."
- *Fix:* Open `setup/setup.log`, find the last `Handler error` entry, and match it to the sections above. Use **Try again** on the install step after fixing the underlying issue.

### Setup wizard — after installation

#### Blank page or 404

- *Symptom:* Site or `/admin` returns a blank page or 404 after the wizard completes.
- *Cause:* Document root is not pointing to `public/`, or URL rewriting is disabled.
- *Fix:*
  - Point your domain's document root to the `public/` directory (recommended).
  - If you cannot change the docroot, confirm a root `.htaccess` was created that redirects to `public/`.
  - Enable **mod_rewrite** on Apache, or configure Nginx using the bundled `.nginx.conf`.

#### Delete the wizard files

- *Symptom:* Security concern or confusion after install.
- *Fix:* Delete **`setup.php`** and the **`setup/`** directory once you have confirmed the site and admin panel work.

### Post-installation issues

These apply after TastyIgniter is installed, whether you used the wizard or the CLI.

#### 404 error page

- *Cause:* `mod_rewrite` is not enabled, `.htaccess` is missing from `public/`, or `RewriteBase` is incorrect for a subdirectory install.
- *Fix:* Enable `mod_rewrite`. Upload or restore `public/.htaccess`. For subdirectory installs, uncomment and set `RewriteBase` in `public/.htaccess`:

```apache
RewriteBase /subdirectory/
```

#### Blank screen (white page)

- *Cause:* PHP fatal error, or `storage/` / `bootstrap/cache/` is not writable.
- *Fix:*
  - Set permissions on `storage/` and `bootstrap/cache/` to **755** or **775**.
  - Review `storage/logs/laravel.log` for the error.
  - Temporarily set `APP_DEBUG=true` in `.env` on a non-production environment to see the error (never leave debug enabled in production).

#### Storefront links not working

- *Cause:* Active theme or required extensions are missing or not enabled.
- *Fix:* Log in to `/admin`, confirm the default theme is installed, and check that required extensions are enabled.

#### Scheduled tasks not running

- *Cause:* Cron job is missing or points to the wrong `artisan` path.
- *Fix:* Add a cron entry that runs every minute:

```bash
* * * * * php /path/to/artisan schedule:run >> /dev/null 2>&1
```

Replace `/path/to/artisan` with the absolute path to the `artisan` file in your TastyIgniter root.

#### Queue jobs not processing

- *Cause:* Queue worker is not running, or `QUEUE_CONNECTION` is set to `sync`.
- *Fix:* Set `QUEUE_CONNECTION=database` (or `redis`) in `.env` and run a queue worker:

```bash
php /path/to/artisan queue:work
```

See the [Laravel Queue documentation](https://laravel.com/docs/queues#running-the-queue-listener) for production setups with Supervisor.

### Getting more help

If you are still stuck:

1. Check `setup/setup.log` (wizard) and `storage/logs/laravel.log` (application).
2. Search or post on the [Community Forums](https://forum.tastyigniter.com).
3. [Join us on Discord](https://tastyigniter.com/discord).
4. Report bugs on the [GitHub issue tracker](https://github.com/tastyigniter/TastyIgniter/issues).

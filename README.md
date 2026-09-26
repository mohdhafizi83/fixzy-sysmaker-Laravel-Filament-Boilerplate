# Fixzy SysMaker — Preview Environment (Laravel 12 + Filament 5)

This repository is the **base template** for the live preview environment used by
[Fixzy SysMaker](https://github.com/mohdhafizi83/fixzy-sysmaker), the visual
database designer that builds your admin back office.

Fixzy SysMaker downloads this template on first run (`download-on-first-run`),
unpacks it into the user data directory, then generates project code on top of it
and boots `php artisan serve` for instant preview.

> This repo is intentionally identical to `resources/preview_env` inside the
> Fixzy SysMaker app (minus `vendor/`, `.env`, and local caches). If the two
> drift, the app's preview stops matching generated output.

## What's inside

| Component | Details |
|---|---|
| **Laravel 12** | Fresh framework, PHP 8.2+ (framework v12.69.x) |
| **Filament 5** | Admin panel (TALL stack), filament/filament v5.8.x |
| **Filament Shield** | Roles & permissions (bezhansalleh/filament-shield ^4.3) |
| **User & Role resources** | Full CRUD with form/table schema split |
| **Native audit trail** | `App\Models\Audit` + `AuditObserver` + `HasAudits` concern |
| **SSO building blocks** | socialite ^5.31, ldaprecord-laravel ^4.0, Google API client |
| **Sanctum** | API tokens (^4.3) |
| **Dev tooling** | Pint, Pail, Sail, PHPUnit/Pest, migration generator |

## Requirements

- PHP 8.2+ (with `pdo_sqlite`)
- Composer 2

## Quick start (standalone)

```bash
git clone https://github.com/mohdhafizi83/FiziSysMaker-Laravel-Filament-Boilerplate.git myapp
cd myapp
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate
php artisan db:seed --class=ShieldSeeder
php artisan serve
```

Default SQLite database — no external services needed.

## How Fixzy SysMaker uses this repo

1. `setupRunner.js` downloads the release archive (`preview-env-vX.tar.gz`)
   matching the app's pinned version and verifies its SHA-256.
2. Unpacked to `~/.fixzy/preview_env` (or Electron userData dir).
3. `.env` is created from `.env.example`, `APP_KEY` generated.
4. Generated project code is overlaid; `php artisan serve` boots the preview.

Do **not** commit `.env`, `vendor/`, `database/*.sqlite`, or compiled caches —
they are runtime artifacts, provisioned per install.

## Version policy

- `main` = latest synced with the app's `resources/preview_env`.
- Releases are tagged `preview-env-vN` with a `preview-env-vN.tar.gz` asset and
  `preview-env-vN.tar.gz.sha256`.
- The app pins a version; bump the pin in `src/core/setupRunner.js` deliberately.

## License

MIT (Laravel base). Fixzy SysMaker itself is non-commercial — see
https://github.com/mohdhafizi83/fixzy-sysmaker.

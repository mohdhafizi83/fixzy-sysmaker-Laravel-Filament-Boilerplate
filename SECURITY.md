# Security Policy

This repository is a **starter template** (Laravel 12 + Filament 5). It is
intended to be forked and used as the basis of your own application. The
template itself ships with no secrets, but a fresh clone is only as safe as
what you put into it before going to production.

## Before deploying to production — rotate / replace everything below

1. **`APP_KEY`** — never reuse a key from a local/dev environment. Generate a
   fresh one on the production host: `php artisan key:generate --show`.
2. **`.env`** — copy `.env.example`, set `APP_ENV=production`,
   `APP_DEBUG=false`, and fill in real DB/session/mail credentials. Never
   commit `.env` (it is gitignored).
3. **Database credentials** — use a least-privilege DB user; do not reuse
   personal or shared credentials.
4. **Default seeded admin account** — if you run the seeders, change the
   default admin password immediately (`php artisan tinker` → update the
   user) or disable seeding in production deploys.
5. **Mail / AWS / third-party tokens** — any `MAIL_*`, `AWS_*` keys added to
   `.env` must be scoped to the deployment and rotated if ever shared.
6. **Session hardening** — set `SESSION_SECURE_COOKIE=true` behind HTTPS and
   use a real `SESSION_DOMAIN`.

## Reporting a vulnerability

If you believe you found a security issue in this template, open a
[private security advisory](https://github.com/mohdhafizi83/fixzy-sysmaker-Laravel-Filament-Boilerplate/security/advisories/new)
rather than a public issue. Note that vulnerabilities in Laravel or Filament
themselves should be reported to those projects per their policies
(laravel.com/security).

## Scope

Supported: the `main` branch of this template. Not supported: forks, or
applications built from the template after you have modified it.

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

## Release assets

Release archives are built from a clean checkout (no `vendor/`, `.env`, caches)
with the tarball root prefixed `preview_env/`:

```bash
git archive --format=tar.gz --prefix=preview_env/ -o preview-env-vN.tar.gz HEAD
sha256sum preview-env-vN.tar.gz > preview-env-vN.tar.gz.sha256
```

Current release: `preview-env-v1`
SHA-256: `3dabf4fa06328f726894a57a357ddf75eb77762fc23fb71c3d44b558bc30beab`

Tarballs are release-only artifacts and are gitignored — never commit them.

## Private-repo note

While this repository is private, the app cannot use the public release download
URL (it 404s). `setupRunner.js` falls back to the GitHub API
(`Accept: application/octet-stream`) and requires a token via `FSM_PREVIEW_TOKEN`
or `GITHUB_TOKEN` (scope: `repo` read). Once this repo is public, the direct
release URL works with no token.

## Bump procedure (keeping the two repos in sync)

1. In Fixzy SysMaker, make the change in `resources/preview_env` first.
2. Sync into this repo (mirror, excluding runtime artifacts):
   ```bash
   rsync -a --delete \
     --exclude vendor --exclude .env --exclude node_modules \
     --exclude 'database/*.sqlite' --exclude '.git' \
     --exclude 'bootstrap/cache/*' --exclude 'storage/**' \
     ../FiziSysMaker/resources/preview_env/ ./
   ```
   Re-apply repo-only files afterwards (`.gitignore`, `.gitattributes`,
   `README.md`) if `--delete` removes them.
3. Verify standalone boot: `composer install && php artisan migrate &&
   php artisan db:seed --class=ShieldSeeder && php artisan serve` →
   `/admin/login` returns 200.
4. Commit, push, then cut a new release tag `preview-env-vN` with the tarball
   and sha256 (see Release assets above).
5. Update `PREVIEW_ENV_VERSION` / `PREVIEW_ENV_SHA256` in the app's
   `src/core/setupRunner.js` and run the app's test suite.

## License

MIT (Laravel base). Fixzy SysMaker itself is non-commercial — see
https://github.com/mohdhafizi83/fixzy-sysmaker.

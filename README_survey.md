# Survey – Back office and API for retail outlet audits

PHP web application (Yii 2) for managing **field surveys / audits in retail outlets** (stores, shops, bars…). It consists of:

- an **administration back office** (AdminLTE theme) for managing periods, schedules, auditors, accounts, stores, products and audits;
- a **REST API** (`api/`, `v1` module) used by the auditors' mobile app, which sends it the data collected in the field (store census, product readings, photos, geolocation).

Intended audience: supervision teams who plan and monitor audit campaigns, and field auditors through the mobile app.

## Main features

### Back office (`controllers/`, `views/`)

- **Dashboard** and administrator authentication.
- **Audit period management** (label, start and end date).
- **Schedule management**: assigning stores to an auditor for a period, start date and time, tracking of actual start and end dates/times, comments; **schedule import** from a file.
- **Auditor management** (name, phone, e-mail, code) and user **account** management (roles, photo).
- **Store management**: detailed record (region, municipality, city, area, urban/rural setting, GPS coordinates, brand, store type, floor area, address, activity type, number of checkouts, refrigeration system, on-site consumption, product categories, owner / chain, contacts, opening hours, storage room, service type, billing system…), visit history by schedule, **CSV export**.
- **Product / item management** (category, code, brand, origin, variant, flavor, packaging, weight, manufacturer, country, alcoholic or not, photo…).
- **Audit management** ("Survey management"): status, type, dates and times, auditor, store, schedule; **CSV export**.

### REST API (`api/modules/v1/controllers/`)

- `compte`: login / logout, refresh, reference lists (domains, parent items, municipalities).
- `store`: list and create stores, photo upload (`uploader`, `uploaders`).
- `product`: product list, categories, countries, filters by brand / category, photo upload.
- `survey`: the auditor's schedules (`plan`), audit creation, entry of product readings (quantity, price, photo, GPS position), opening / reopening / closing / completion of an audit, editing and deletion.

### Import scripts (`import/`)

Standalone PHP scripts for importing products and stores into the database from CSV files.

## Tech stack

- **PHP** ≥ 5.6 (Docker image provided: PHP 7.1 + Apache)
- **Yii 2** (`yiisoft/yii2` ~2.0.14), "basic" template with a separate API entry point
- `yiisoft/yii2-bootstrap`, `yiisoft/yii2-swiftmailer`
- `dmstr/yii2-adminlte-asset` (AdminLTE theme for the back office)
- Kartik widgets: `yii2-widget-datepicker`, `yii2-widget-select2`, `yii2-widget-timepicker`
- MySQL database (through the Yii configuration)
- Tests: Codeception

## Project structure

```
api/                 # REST API application (entry point api/index.php, config/api.php, modules/v1)
assets/              # Yii asset bundles
commands/            # Console commands
config/              # Configuration (web.php, console.php, db.php, params.php, test*.php)
controllers/         # Back-office controllers
import/              # CSV import scripts (products, stores)
mail/                # E-mail templates
models/              # ActiveRecord models and search models (Store, Product, Planning,
                     #   Periode, Auditeur, Compte, AuditStore, AuditProduit, Domaine…)
runtime/             # Files generated at runtime
tests/               # Codeception tests
uploads/             # Uploaded files and photos
views/               # Back-office views (AdminLTE layouts included)
web/                 # Web entry point (index.php) and public resources
docker-compose.yml   # PHP/Apache Docker environment
Vagrantfile, vagrant/ # Vagrant environment
```

## Installation and running

1. Install the dependencies:

   ```bash
   composer install
   ```

2. Create a MySQL database and set the connection in `config/db.php` (and `config/test_db.php` for the tests).

3. Start the application:

   - with the Yii built-in server:

     ```bash
     php yii serve --docroot=web          # back office
     php -S localhost:8081 -t api         # REST API
     ```

   - or with Docker:

     ```bash
     docker-compose run --rm php composer install
     docker-compose up -d
     ```

     The container exposes the application on port `8000` (`http://127.0.0.1:8000`).

4. Run the tests:

   ```bash
   vendor/bin/codecept run
   ```

## Configuration

No `.env.example` file is provided. Configuration is done in the PHP files of `config/` (database connection in `db.php`, application parameters in `params.php`) and in `api/config/api.php` for the API. The scripts in `import/` contain their own connection settings, which must be adjusted before use. Remember to disable `YII_DEBUG` / `YII_ENV=dev` in `web/index.php` and `api/index.php` in production.

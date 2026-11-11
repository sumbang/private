# Health'ys – Back office and API (Yii 2)

PHP back end of the **Health'ys** healthcare platform. The project includes:

- a web administration **back office** (patients, doctors, partners, subscriptions,
  digital health cards, consultations, vaccination…) for platform administrators
  and healthcare facilities;
- a **REST API** (`api/`, `v1` module) consumed by the Health'ys mobile apps
  (patients and doctors).

The project is built from the *Yii 2 Basic Project Template*.

## Main features

**Web back office** (menus depend on the logged-in user's role)
- Dashboard
- Administration: patients, doctors, resellers, accounts, profiles
- Entities: partners (hospitals, pharmacies, laboratories…), directory, doctor assignment,
  companies, staff and medical staff, nurses
- Subscriptions: plans, subscription codes, sign-ups, account top-ups, payments,
  sales and commissions
- Digital health cards: creation, assignment, activation, sale, PDF export / printing with QR code
- Settings: specialties, countries, news
- Operations: consultations, medical appointments, medical certificates,
  vaccination programs and schedules, recorded vaccines
- PDF consultation reports, password reset by email,
  SMS sending (Twilio), online sign-up / payment pages

**REST API v1** (`api/modules/v1/controllers`)
- `compte`: login / logout, account creation and update, password reset,
  profile, medical parameters and past medical history, emergency information,
  document upload, subscriptions and sign-ups, patient and doctor search
- `consultation`: consultation creation and update, medical record, reports,
  certificates, documents, doctor-side and patient-side consultations
- `vaccin`: vaccines and vaccination programs
- `meeting`: appointments (creation, confirmation, rejection)
- `partner`: partners and companies
- `new`: news

## Tech stack

- **PHP** (>= 5.6 per `composer.json`; Docker image `yiisoftware/yii2-php:7.1-apache`)
- **Yii 2** (`yiisoft/yii2 ~2.0.14`), **MySQL** database
- Admin UI: `yiister/yii2-gentelella`, `hail812/yii2-adminlte3`,
  `ramosisw/yii2-material-dashboard`, `yiisoft/yii2-bootstrap`, `yiisoft/yii2-jui`
- PDF: `kartik-v/yii2-mpdf`; QR codes: `chillerlan/php-qrcode`, `2amigos/qrcode-library`
- Email: `yiisoft/yii2-swiftmailer`; SMS: `twilio/sdk`
- Tests: Codeception (`codeception.yml`, `tests/`)

## Project structure

```
api/            # REST API application (config/api.php, modules/v1/controllers)
assets/         # Asset bundle definitions
commands/       # Console commands
config/         # Configuration (web.php, console.php, db.php, params.php, test*.php)
controllers/    # Web back-office controllers
mail/           # Email templates
messages/       # Translations
migrations/     # Database migrations
models/         # ActiveRecord models and search models
views/          # Back-office views (one per module + layouts/)
web/            # Web entry point (index.php) and public assets
widgets/        # Widgets
uploads/        # Uploaded files
tests/          # Codeception tests (unit, functional, acceptance)
docker-compose.yml, Vagrantfile   # Development environments
```

## Installation and running

Install dependencies:

```bash
composer install
```

Database: create a MySQL database, set the connection in `config/db.php`, then:

```bash
php yii migrate
```

Running locally:

```bash
php yii serve            # back office at http://localhost:8080
```

With Docker:

```bash
docker-compose run --rm php composer install
docker-compose up -d     # app available at http://127.0.0.1:8000
```

Tests:

```bash
vendor/bin/codecept run
```

## Configuration

No `.env.example` file is provided: configuration is done in the PHP files under `config/`:

- `config/db.php`: MySQL database connection
- `config/web.php`: back-office configuration (cookie validation key, mailer, URL)
- `api/config/api.php`: API configuration (REST rules, mailer)
- `config/params.php`: application parameters

Do not commit passwords or keys in these files; use environment-specific values.

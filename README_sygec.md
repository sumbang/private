# SYGEC

Web application for managing staff leave, absences and permissions within an organization. It is intended for human resources departments and line managers: tracking employees and the organizational chart, issuing and approving leave decisions, tracking leave periods taken, suspensions, interruptions and cancellations, and generating the related documents.

## Main features

- **Dashboard** and dynamic menu based on the user's permissions
- **Organizational chart**: directorates, departments, services, jobs
- **Human resources**: employee records (category, grade step, contract, marital status…), history
- **Leave and permissions**:
  - leave decisions per fiscal year: creation, approval, return to work, cancellation, suspension, reminder
  - leave periods (leave actually taken), suspensions, interruptions, cancellations
  - one-off absences and absence types
  - certified copies (recipients of decisions)
  - generation of decisions and documents in PDF format
- **Import / export**: import of decisions and employees from CSV files, CSV export of decisions, absences and employees
- **Documents and downloads**: attachment upload and download area
- **Security**: users, roles, permissions, settings; login and password reset by e-mail
- **Console commands** for bulk processing (imports, generation, e-mail sending, data fixes)

## Tech stack

- **PHP** `>= 8.0`
- **Yii 2** (`yiisoft/yii2 ~2.0.14`, "basic" template)
- UI: Bootstrap (`yii2-bootstrap`), AdminLTE theme (`dmstr/yii2-adminlte-asset`), jQuery UI
- Kartik widgets: date picker, date range, Select2, PDF generation (`kartik-v/yii2-mpdf`)
- E-mails: `yii2-swiftmailer`
- Relational database (MySQL / MariaDB via PDO) with Yii migrations
- Tests: Codeception
- French-language interface (`language => 'fr'`)

## Project structure

```
assets/        # Asset bundles (CSS/JS)
commands/      # Console commands (CSV imports, exports, generation, e-mails)
config/        # Web, console and test configuration (db.php must be created locally)
controllers/   # Web controllers (employees, leave decisions, leave periods, absences, roles…)
mail/          # E-mail templates
migrations/    # Database migrations
models/        # ActiveRecord models and search models (*Search)
views/         # Per-controller views and AdminLTE layouts
web/           # Entry point (index.php) and public resources
widgets/       # Custom widgets
tests/         # Codeception tests
old/           # Legacy database update scripts
```

## Installation and running

Prerequisites: PHP 8, Composer, a MySQL/MariaDB database server.

```bash
composer install
```

Create the `config/db.php` file (not tracked by Git), for example:

```php
<?php
return [
    'class' => 'yii\db\Connection',
    'dsn' => 'mysql:host=localhost;dbname=<database_name>',
    'username' => '<user>',
    'password' => '<password>',
    'charset' => 'utf8',
];
```

The repository only contains a few incremental migrations (directorates, services, decision updates): the base schema must be imported beforehand. Then apply the migrations and start the development server:

```bash
php yii migrate
php yii serve      # http://localhost:8080
```

With Docker (`yiisoftware/yii2-php` image):

```bash
docker-compose run --rm php composer install
docker-compose up -d   # http://127.0.0.1:8000
```

Console command examples:

```bash
php yii employe/import <file.csv>
php yii decisionconges/import <file.csv> <fiscal_year>
php yii export-data/decision
php yii export-data/absence
```

Tests:

```bash
vendor/bin/codecept run
```

## Configuration

There is no `.env.example` file. Configuration lives in `config/`:

- `config/db.php`: database connection (to be created, ignored by Git)
- `config/web.php` / `config/console.php`: application components (cookie validation key, SMTP mailer…) — fill in your own values
- `config/params.php`: application parameters

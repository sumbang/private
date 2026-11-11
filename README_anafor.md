# ANAFOR: production, inventory, sales and collections management

Commercial and production management web application developed for ANAFOR. It tracks site production (quantities in m³), stock and stock movements, customer quotations and invoicing, and then payment collection. The project also provides a **REST API (v1)** that exposes the same data to external clients, such as a mobile app.

Intended audience: internal teams (operators, approvers, sales and production managers) working per site, base and branch office.

## Main features

- **Authentication**: sign-in, sign-out, password reset by email.
- **Bilingual interface** French / English, with a language selector.
- **Permission-based dynamic menus**: menu entries come from the database and depend on the user's role. They are grouped into "Basic processing", "Production operations", "Sales operations" and "Reporting operations".
- **Reference data**: regions, cities, sites, units, product families (categories), products, prices, fiscal years, roles, users, settings, taxes, customer types and payment methods.
- **Production**: production inventories by fiscal year, site and product (opening stock, target in m³), with validation and cancellation.
- **Stock and movements**: stock receipts and issues with validation and cancellation, summaries, stock status and export.
- **Sales and invoicing**: quotation, approval or rejection of a request, **PDF** invoice generation, summaries, status and export.
- **Collections**: recording payments, validation, cancellation and calculation of the outstanding balance.
- **Dashboard** with indicators: pending invoices and alerts.
- **REST API v1** (`api/`): resources `user` (login, resetpwd), `client`, `facture` (valider, rejeter, buildfacture, search), `cotation`, `commentaire`, `document`, `validation`, `stock`, `produit`, `famille`, `unite`, `taxe`, `ville`, `sites`, `roles`, `exercice`, `recouvrement`, `paiementmode`, `natureclient` and `setting`.

## Tech stack

- **PHP** >= 5.4. A PHP 7.1 + Apache Docker image is provided by the Yii template.
- **Yii 2** (~2.0.14), "basic" template, plus a separate API application that uses `yii\rest`.
- **AdminLTE** (`dmstr/yii2-adminlte-asset`), **yii2-bootstrap** and **yii2-jui**.
- **Kartik** widgets: alert, select2, datepicker, date-range and **mPDF** (PDF generation).
- Email: **yii2-swiftmailer** and **SendGrid** (`wadeshuler/yii2-sendgrid`).
- Relational database (MySQL) via ActiveRecord.
- Tests: **Codeception**.

## Project structure

```
api/           REST API (entry point api/index.php, config, v1 module and its controllers)
assets/        Asset bundles
commands/      Console commands
config/        Web and console application configuration
controllers/   Back-office controllers (Facture, Stocks, Production, Recouvrement, reference data…)
messages/      Translations (fr-FR, en-US)
models/        ActiveRecord models, search models and forms
views/         Views (AdminLTE layouts, screens per entity)
web/           Public web root
widgets/       Reusable widgets
tests/         Codeception tests
```

## Installation and running

1. Install the dependencies:
   ```bash
   composer install
   ```
2. Configure the database in `config/db.php` and the API settings in `api/config/api.php`.
3. Start the application:
   - with the Yii built-in server:
     ```bash
     php yii serve
     ```
   - or with Docker:
     ```bash
     docker-compose up -d
     ```
     The application is then available at `http://localhost:8000`.
4. For the API, point a virtual host to the `api/` folder (entry point `api/index.php`).
5. Run the tests:
   ```bash
   vendor/bin/codecept run
   ```

## Configuration

The repository does not contain a `.env.example` file. Configuration is done in the PHP files of the `config/` folder (`db.php`, `params.php`, `web.php`) and in `api/config/api.php`. The email delivery service is also configured there. Do not commit passwords or API keys in these files.

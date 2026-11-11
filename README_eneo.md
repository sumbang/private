# eneo: back-office and API for field meter reports

Web application for managing the **field reports** produced by agents who read and inspect electricity meters. It provides:

- a **back-office** (administration interface) for supervisors: browsing field reports, managing users, regions, agencies and data files;
- a **webservice** for the agents' mobile application: authentication, download of the region's reference data file, submission of field reports.

This repository is an earlier, simpler version of the same codebase as `eneo-web` (same Yii 2 back-office, a smaller webservice, plus an experimental REST module).

Intended users: supervision teams (back-office) and field agents (through the mobile application that calls the webservice).

## Main features

### Back-office (Yii 2)
- User login / logout (AdminLTE layout).
- **Field data** (`rapport`): list, search, view, edit, delete and **CSV export** (anomaly, usage, index read, amperage, latitude/longitude, location, meter, observations).
- **Users**: management of agents (registration number, name, region, agency, status, role).
- **Regions** and **Agencies**: geographic reference data.
- **Data updates** (`datas`): upload of data files per region (stored in `web/uploads/`), made available to the mobile application.

### Webservice (`webservice/controller.php`)
Single entry point driven by the `action` parameter:
- `login`: agent authentication by registration number (matricule);
- `getdata`: returns the latest data file for the agent's region;
- `setdata`: records a field report (anomaly, usage, index read, amperage, GPS position, location, meter, observations, registration number, date).

### REST API (`api/`)
Separate Yii 2 application (`api/index.php`, config in `api/config/api.php`) exposing a `v1/user` REST controller with `POST login` and `POST resetpwd` actions (JSON input, CORS enabled). This module appears to have been adapted from another project and is not fully configured for this one.

## Tech stack

- **PHP** >= 5.4 (Docker image provided: PHP 7.1 + Apache)
- **Yii 2** (~2.0.14), "basic" project template
- **yii2-bootstrap**, **AdminLTE** (`dmstr/yii2-adminlte-asset`), **yii2-swiftmailer**
- **MySQL** database
- Tests: **Codeception**

## Project structure

```
api/           Separate REST application (module v1, UserController)
assets/        Yii asset bundles
commands/      Console commands
config/        Configuration (web, console, database, params, tests)
controllers/   Controllers: Site, Rapport, Users, Region, Agence, Datas
models/        ActiveRecord models and search models
views/         Views (AdminLTE layouts, CRUD per entity)
webservice/    Plain PHP API used by the mobile application
web/           Public web root (index.php, uploads)
widgets/       Reusable widgets
tests/         Codeception tests
vagrant/       Vagrant provisioning (from the Yii template)
```

## Installation and running

1. Install dependencies:
   ```bash
   composer install
   ```
2. Configure the MySQL connection in `config/db.php` and in `webservice/database.php` (used by the webservice).
3. Start the application:
   - with the built-in server:
     ```bash
     php yii serve
     ```
   - or with Docker:
     ```bash
     docker-compose up -d
     ```
     The application is then available at `http://localhost:8000`.
4. Run the tests:
   ```bash
   vendor/bin/codecept run
   ```

## Configuration

The project does not ship a `.env.example` file. Configuration lives in the `config/` folder (`db.php`, `params.php`, `web.php`), in `webservice/database.php` and in `api/config/api.php`. Do not commit passwords or other credentials in these files.

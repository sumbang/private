# eneo-web: back office and API for field meter readings

Web application for managing the **field reports** produced by agents in charge of reading and inspecting electricity meters. It serves as both:

- a **back office** (admin interface) for supervisors: viewing reports, managing users, regions, branches and data files;
- a **web service** for the agents' mobile app: authentication, downloading reference data, submitting field reports.

Intended audience: supervision teams (back office) and field agents (through the mobile app that consumes the web service).

## Main features

### Back office (Yii 2)
- User sign-in / sign-out, with different menus depending on the role.
- **Dashboard** with "Field performance" and "Anomaly volumes" charts.
- **Field reports**: list, search, view, edit, delete and **CSV export** (anomaly, usage, meter reading, amperage, latitude/longitude, location, meter, remarks, employee ID, operation date, entry distance).
- **Users**: agent management (employee ID, name, phone, region, branch, status, role).
- **Regions** and **Branches**: management of geographic reference data.
- **Data update**: upload of data files per region (stored in `web/uploads/`), made available to the mobile app.

### Web service (`webservice/controller.php`)
Single entry point driven by the `action` parameter:
- `login`: agent authentication by employee ID;
- `getdata`: retrieval of the data file for the agent's region;
- `setdata`: recording of a field report (anomaly, usage, reading, amperage, GPS position, meter, remarks), with a check that the meter matches the contract;
- `amperage`, `anomalies`: reference lists;
- `compteur`, `grand`, `recherche`, `recherche1`: search for meters, contracts, key accounts and delivery points;
- `token`: device token management.

### Import scripts
- `compteur_to_contrat.php`: import of a CSV file mapping meters to contracts.
- `grand_compte.php`: import of a CSV file of key account reference data.

## Tech stack

- **PHP** >= 5.4 (Docker image provided: PHP 7.1 + Apache)
- **Yii 2** (~2.0.14), "basic" template
- **yii2-bootstrap**, **AdminLTE** (`dmstr/yii2-adminlte-asset`), **yii2-swiftmailer**
- **Kartik** widgets (date-range, datepicker)
- **MySQL** database
- Tests: **Codeception**

## Project structure

```
assets/        Yii asset bundles
commands/      Console commands
config/        Configuration (web, console, database, parameters)
controllers/   Controllers: Site, Rapport, Users, Region, Agence, Datas
models/        ActiveRecord models and search models
views/         Views (AdminLTE layouts, CRUD per entity, dashboard)
webservice/    PHP API consumed by the mobile app
web/           Public web root (index.php, uploads)
widgets/       Reusable widgets
tests/         Codeception tests
*.php (root)   CSV import scripts
```

## Installation and running

1. Install the dependencies:
   ```bash
   composer install
   ```
2. Configure the MySQL database connection in `config/db.php` (and in `webservice/` for the API).
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

The project does not provide a `.env.example` file. Configuration is done in the files of the `config/` folder (`db.php`, `params.php`, `web.php`) and in the connection files of the `webservice/` folder. Do not commit passwords in these files.

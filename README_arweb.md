# arweb: back office and API for tracking equipment maintenance

arweb is a web application for **tracking maintenance operations** on equipment installed at customer sites. Equipment is organized by customer, then by address, room and item. The application includes:

- a **back office** for administrators: management of technicians, maintenance records, checklists and users, plus access to the audit trail;
- a **REST API (v1)** for the technicians' mobile app: code-based login, equipment lookup, recording of interventions with before and after photos, and statistics.

Intended audience: managers and administrators (back office) and maintenance technicians (through the API).

## Main features

### Back office
- User login, logout and profile.
- **Home**: dashboard with charts (Chart.js, Google Charts).
- **Technicians**: management of technicians assigned to a customer address, with a secret code and a lead-technician flag.
- **Maintenance**: list, view, edit, delete and **export** completed maintenance records. Each record stores the item, the technician, the date, the nature, the type, a comment and the before and after images.
- **Maintenance list**: catalog of maintenance operations by category and nature.
- **Audit trail**: log of technician actions (date, action, message, device).
- **Users**: management of back-office accounts.

### REST API v1 (`modules/api/v1`)
- Authentication: `login`, `logout`, `secret`, `resetpwd`, `newpass`.
- Data: `pieces`, `equipement`, `listes`, `list-maintenance`, `techniciens`.
- Technician management: `new-tech`, `remove-tech`.
- Interventions: `new-maintenance`, photo upload (`uploader`, `uploader1`).
- Statistics: `stats`.

### Data model
The main entities are: customers, addresses, rooms, items (equipment with a photo and specifications), categories (with the maintenance interval), groups, roles, technicians, maintenance records, maintenance lists and the audit trail.

## Tech stack

- **PHP** >= 5.6
- **Yii 2** (~2.0.14), "basic" template, with a REST API module (`yii\rest`)
- **Bootstrap 4** (`yii2-bootstrap4`) and **AdminLTE** (`dmstr/yii2-adminlte-asset`, `yidas/yii2-adminlte`)
- Charts: **Chart.js** (`practically/yii2-chartjs`) and **Google Charts** (`scotthuangzl/yii2-google-chart`)
- **yii2-swiftmailer** for sending e-mails
- Relational database (MySQL) through ActiveRecord
- Tests: **Codeception** 4

## Project structure

```
assets/            Asset bundles
commands/          Console commands
config/            Configuration (web, console, database, parameters)
controllers/       Back-office controllers (Site, Technicien, Maintenance, Maintenancelist, Piste, Compte)
models/            ActiveRecord models and search models
modules/api/v1/    REST API module used by the mobile app
views/             Views (AdminLTE layouts, per-entity screens)
web/               Public web root
widgets/           Reusable widgets
tests/             Codeception tests
```

## Installation and running

1. Install the dependencies:
   ```bash
   composer install
   ```
2. Configure the database in `config/db.php` and the e-mail service in `config/web.php`.
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
4. The API is exposed by the `apiV1` module through the REST rules defined in `config/web.php` (pretty URLs enabled).
5. Run the tests:
   ```bash
   vendor/bin/codecept run
   ```

## Configuration

The repository does not include a `.env.example` file. Configuration lives in the files of the `config/` folder (`db.php`, `params.php`, `web.php`). Do not commit passwords or SMTP credentials to these files. Keep them instead in a local file not tracked by Git or in environment variables.

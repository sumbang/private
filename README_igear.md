# I-GEAR: management of a car garage and a vehicle fleet

I-GEAR is a management web application for an automotive business: repair workshop, sales of new and used vehicles, vehicle rental and spare parts management. It covers the sales cycle (quotes, invoices, credit notes), purchasing from suppliers, inventory, cash register, staff and calendar.

Intended audience: the administrative, sales and technical staff of a garage or car dealership.

## Main features

- **Authentication and permissions**: sign-in, sign-out, password reset, user profile. Roles and permissions define the menus available to each user.
- **Dashboard** and **statistics** pages. Summary screens exist per area: cash register, customers, quotes, invoices, suppliers, rental, inventory and vehicles.
- **Vehicles**: new or used vehicle records (purchase and sale price, VAT, margin VAT, margin, discount), registrations, makes, types, body styles, colors and fuel types.
- **Workshop**: labor (rates, VAT, accounting accounts), packages and jobs (work orders linked to a registration).
- **Items and inventory**: items, item families, ingredients and stock management.
- **Purchasing**: suppliers and orders (approval, rejection, quantities ordered and delivered, payment).
- **Sales**: customers, quotes (approval, closing, conversion to invoice), customer and supplier invoices (status, closing, **PDF** generation) and credit notes.
- **Rental**: rental contracts (dates, mileage and fuel at departure and return, insurance, price excluding VAT and VAT) and insurance policies.
- **Cash register**: cash movements (receipts and disbursements).
- **Fixed assets**: fixed asset tracking.
- **Staff and calendar**: staff records (position, employee ID, seniority), user accounts, calendar of assigned tasks, zones.
- **Configuration** and application settings.

## Tech stack

- **PHP** >= 5.4. A PHP + Apache Docker image is provided by the Yii template.
- **Yii 2** (~2.0.14), "basic" template.
- **AdminLTE** (`dmstr/yii2-adminlte-asset`) and **yii2-bootstrap**.
- **Kartik** widgets: select2 and **mPDF** (PDF document generation).
- **yii2-swiftmailer** for sending emails.
- Relational database (MySQL) via ActiveRecord.
- Tests: **Codeception**.

## Project structure

```
assets/        Asset bundles
commands/      Console commands
config/        Configuration (web, console, database, parameters)
controllers/   Controllers (one per area: Vehicule, Devis, Facture, Location, Stock…)
models/        ActiveRecord models, search models and forms
views/         Views (AdminLTE layouts, CRUD screens per entity)
web/           Public web root
widgets/       Reusable widgets
tests/         Codeception tests
```

## Installation and running

1. Install the dependencies:
   ```bash
   composer install
   ```
2. Configure the database in `config/db.php`.
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
4. Run the tests:
   ```bash
   vendor/bin/codecept run
   ```

## Configuration

The repository does not contain a `.env.example` file. Configuration is done in the files of the `config/` folder (`db.php`, `params.php`, `web.php`). Do not commit passwords in these files.

# eneo-mob: Customer Overview, the field agents' Android app

Android application ("Customer Overview") used by field agents who read and inspect electricity meters. It lets an agent look up a customer or supply point, see the customer's consumption, bills, unpaid amounts and prepaid tokens, and **report field anomalies** (with GPS position), including offline with later synchronisation.

It is the mobile client of the `eneo-web` back-office: it authenticates against, downloads reference data from and sends field reports to the `webservice/controller.php` webservice of that project.

Intended users: field agents (meter readers and inspectors).

## Main features

- **Login** with registration number (matricule) and password (password hashed with SHA-1 before it is sent); the session is kept in `SharedPreferences`.
- **Home screen** (grid): Search, Anomalies, Synchronisation, Logout.
- **Data update**: downloads the data file for the agent's region (`getdata` action) and imports it (CSV via OpenCSV, or XLS via JExcelApi) into a local SQLite database (customers, meters, supply points, 12 months of consumption, category, subscription, unpaid amounts, amperage...).
- **Search** by contract number, customer name or meter number, plus search by supply point (`recherche`, `recherche1`, `compteur` actions).
- **Customer details**: customer information, key-account check (`grand` action), totals of unpaid consumption and fraud amounts.
- **Bills**: list of the latest bills for a contract and **consumption chart** (MPAndroidChart).
- **Unpaid bills** and **prepaid tokens** lists for a customer/meter, fetched from external billing services.
- **Anomaly report**: anomaly chosen from a predefined list (bilingual French/English codes such as FP, TK, FD, NT, IN, SC...), usage, meter, index read, amperage, observations, location reference and GPS coordinates. Sent right away (`setdata` action) when online, otherwise stored locally.
- **Synchronisation**: sends the anomalies stored offline and removes them from the local database once accepted by the server.

## Tech stack

- **Java**, Android SDK (compileSdk / targetSdk 29, minSdk 21), Android Gradle Plugin 3.6.3
- AndroidX (AppCompat, ConstraintLayout, DrawerLayout, ViewPager, CoordinatorLayout), Material Components
- **OkHttp** 3.11 (HTTP calls), **OpenCSV** 3.9 and **JExcelApi** (`libs/jxl.jar`) for importing data files
- **MPAndroidChart** 3.1.0 (charts), **Clans FAB** (floating action buttons), Google Play Services Maps
- Local **SQLite** storage

## Project structure

```
app/
  build.gradle               Module configuration and dependencies
  libs/jxl.jar               JExcelApi library (XLS reading)
  src/main/
    AndroidManifest.xml      Activities and permissions (internet, location, storage, camera)
    java/cm/eneo/
      MainActivity           Login
      HomeActivity           Main menu
      UpdateActivity         Download and import of the region data file
      SearchActivity, SupplyActivity, SearchResultActivity   Search
      DetailCustomerActivity Customer details
      ListeBillActivity, GrapheActivity  Bills and consumption chart
      ImpayeActivity, ListTokenActivity  Unpaid bills, prepaid tokens
      AlerteActivity         Anomaly report
      Synchronisation        Sending of offline reports
      HttpQuery              Network calls (webservice and billing services)
      Database, Request      SQLite schema and data access
      GPSTracker, LocationTrack  Geolocation
    res/                     Layouts, strings (anomaly list), styles, fonts
build.gradle, settings.gradle  Gradle project configuration (EneoCustomerOverview)
```

## Installation and running

Prerequisites: Android Studio (or JDK 8 + Android SDK 29).

```bash
# Debug build
./gradlew assembleDebug

# Install on a connected device / emulator
./gradlew installDebug

# Unit tests
./gradlew test
```

You can also open the project in Android Studio and run the `app` module.

## Configuration

There is no `.env.example` file. The server URLs (the `eneo-web` webservice and the external billing/token services) are defined as constants in `app/src/main/java/cm/eneo/HttpQuery.java`; set them for your own environment. The allowed cleartext domains are listed in `app/src/main/res/xml/network_security_config.xml`. Do not commit credentials in the source code.

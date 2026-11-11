# E-survey – Mobile app for field auditors

Flutter mobile app (Android / iOS) for **field auditors** responsible for carrying out **surveys / audits in retail outlets**. It lets them view their audit schedule, record stores, log the products on display (prices, quantities, photos) and send this geolocated data to a REST API.

It is the mobile client of the Yii 2 back office and API of the `survey` project (resources `compte`, `stores`, `products`, `surveys`).

## Main features

- Auditor **login / logout** (token stored locally via `shared_preferences`), data refresh.
- Tab navigation: **Planning**, **Surveys**, **Stores**, **Products**, **Settings**.
- **Planning**: view the audits scheduled for the auditor.
- **Surveys (audits)**: create an audit, open, reopen, close and complete an audit, edit and delete; enter product readings for a store.
- **Stores**: list, detailed record and census form for a new store (administrative location through region / municipality reference lists, outlet characteristics…).
- **Products**: list, search, add and edit items (categories, countries, attributes), with **photos** (camera or multiple selection from the gallery) uploaded to the server.
- **Geolocation** of stores, audits and product readings (`geolocator`).

## Tech stack

- **Flutter** / **Dart** (SDK `>=2.7.0 <3.0.0`, pre-null-safety code)
- Main dependencies: `http`, `shared_preferences`, `geolocator`, `image_picker`, `multi_image_picker`, `cached_network_image`, `flutter_typeahead`, `simple_autocomplete_formfield`, `dropdown_formfield`, `flutter_datetime_picker`, `time_range`, `flutter_slidable`, `flutter_staggered_grid_view`, `webview_flutter`, `fluttertoast`, `font_awesome_flutter`
- App icons generated with `flutter_launcher_icons`
- Android: minSdk 16

## Project structure

```
lib/
├── main.dart        # Application entry point
├── page/            # Screens (login, home, audit, audit completion/reopening, new store,
│                    #   store details, new/edit product, new audit…)
├── fragment/        # Home screen tabs (planning, audit forms, stores,
│                    #   products, audited products, settings)
├── form/            # Forms (login, store, product, audit, audit completion, reopening)
├── model/           # Data models (Store, Product, Audit, AuditProduit, Survey, Domaine,
│                    #   Categorie, Pays, Item)
└── config/          # Configuration (API and image server URLs in Setting.dart), screen sizes
img/                 # App images
android/, ios/       # Native projects
```

## Installation and running

Prerequisites: a Flutter SDK compatible with Dart 2.x (a version earlier than Dart 3) and an Android / iOS emulator or device.

```bash
flutter pub get
flutter run
```

Build a production release:

```bash
flutter build apk     # Android
flutter build ios     # iOS
```

## Configuration

No `.env` file is used. The REST API and image server URLs are defined in `lib/config/Setting.dart` (`apiracine`, `serveurracine`, `serveurimage`); adjust them to your deployment of the `survey` back office before running the app.

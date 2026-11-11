# Healthys Vendeur

Flutter mobile app of the **Healthys** family, intended for **resellers / sales agents**
of the Healthys network. It lets them register new patients, activate their accounts
(subscriptions) and register partner companies, using the Healthys back-end API.

It complements the other apps of the ecosystem: `healthys_patients` / `healthys_patient`
(patients) and `helthysfront` (facilities and doctors).

## Main features

- Reseller **login**, password reset and password change, "My account" page.
- **Register a patient**: entry of identity, contact details (phone, email, address, city),
  occupation, ID document, etc., with photo capture; "New patient" confirmation screen.
- **Activate an account**: entry or **QR code scan** of the patient ID number, choice of subscription and
  payment method, reseller code and payment reference.
- **Register a company**: company name, business activity, manager, contact details, city and district.
- **Multilingual**: French, English and Spanish translation files (`locale/i18n_*.json`).

## Tech stack

- **Language**: Dart (SDK `>=2.7.0 <3.0.0`)
- **Framework**: Flutter (Android, iOS, web)
- **Main dependencies**: `http`, `shared_preferences`, `qrscan`, `qr_flutter`, `image_picker`,
  `camera`, `webview_flutter`, `flutter_webview_plugin`, `cached_network_image`, `fluttertoast`,
  `flutter_datetime_picker`, `intl_phone_field`, `flutter_pdfview`
- **Android application ID**: `cm.healthys_vendeur`

## Project structure

```
lib/
├── main.dart        # App entry point
├── config/          # Configuration (API URL), translations, image compression
├── page/            # Screens (login, home, new patient, activation, company, account…)
├── form/            # Forms (login, patient, activation, company, password)
├── fragment/        # Home fragment
└── model/           # Data models (Compte, Profil, Menu…)
locale/              # Translation files (fr, en, es)
img/                 # Images and icons
android/, ios/, web/ # Native projects and web target
```

## Installation and running

Prerequisites: a Flutter SDK compatible with Dart 2 (constraint `>=2.7.0 <3.0.0`), Android Studio / Xcode.

```bash
flutter pub get
flutter run                 # run on a device or an emulator
flutter build apk           # build the Android APK
flutter build ios           # build the iOS app (on macOS)
```

## Configuration

The project does not include a `.env.example` file. The back-end server addresses are defined in
`lib/config/Setting.dart` and must be adapted to your environment before building.

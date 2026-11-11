# Health'ys Médecin – Mobile app for doctors

Flutter app (`healthys_medecin`, version `1.0.1+12`) of the **Health'ys** healthcare platform,
intended for doctors. It lets them view a patient's medical record (via the patient's digital
health card with QR code), conduct and record consultations, manage their appointments and
track their patients' vaccinations.

The app consumes the Health'ys platform REST API.

## Main features

- Language selection (French, English, Spanish), splash screen
- Login, account creation, activation, password reset and password change
- Profile selection, app lock with a passcode (`flutter_app_lock`)
- Doctor's digital professional card (QR code)
- Scanning the QR code of a patient's health card to access their record
- Creation of patients and medical records
- Consultations: multi-step creation, history, details, follow-up, signature
- Patient's past medical history, blood group / electrophoresis
- Medical certificates and medical reports (PDF display)
- Appointments: creation, viewing, editing
- Vaccination record and schedule, adding vaccines
- Patient documents (image and file upload)
- Payment request (payment method and reference), subscriptions and sign-ups
- News (Health News)
- Partner directories: hospitals, pharmacies, laboratories, imaging centers,
  insurers, banks, insurance brokers
- Account settings, privacy and security, terms of use

## Tech stack

- **Flutter** / **Dart** (SDK constraint `>=2.18.4 <3.0.0`)
- Networking: `http`, `dio`
- Local storage: `shared_preferences`
- Internationalization: JSON files in `locale/` loaded by `lib/config/all_translations.dart`
- QR code: `qr_flutter` (generation), `mobile_scanner` (scanning)
- Media and documents: `image_picker`, `multi_image_picker`, `file_picker`,
  `flutter_image_compress`, `flutter_pdfview`, `image_gallery_saver`
- Signature: `flutter_signature_pad`, `syncfusion_flutter_signaturepad`
- Other: `flutter_app_lock`, `webview_flutter_plus`, `intl_phone_field`,
  `cached_network_image`, `flutter_slidable`, `url_launcher`, `permission_handler`

## Project structure

```
lib/
├── config/      # Settings (Setting.dart), translations, singleton, image compression
├── fragments/   # App sections (dashboard, consultations, appointments, vaccination, directories…)
├── forms/       # Forms (login, patient, consultation, appointment, certificate, payment…)
├── lock/        # App lock screen
├── models/      # Data models (patient, doctor, consultation, vaccine, subscription…)
├── pages/       # App screens
└── main.dart
img/             # Images and icons
locale/          # JSON translations (fr, en, es)
android/ ios/ web/   # Platform projects
test/            # Tests
```

## Installation and running

Prerequisites: a Flutter SDK compatible with Dart 2.18 / 2.19 (the project predates Dart 3),
an emulator or a connected device.

```bash
flutter pub get          # install dependencies
flutter run              # run the app
flutter build apk        # Android build
flutter build ios        # iOS build (on macOS)
```

App icons: `flutter pub run flutter_launcher_icons:main`.

## Configuration

- No `.env.example` file is provided.
- The API and file server addresses are defined in `lib/config/Setting.dart`.

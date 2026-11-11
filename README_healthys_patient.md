# Healthys Patient (Heatly)

Flutter mobile app of the **Healthys** family, intended for **patients**. It lets them keep their
health information online (digital health card, health booklet, medical history,
vaccination record), browse directories of healthcare professionals and facilities, and manage
their appointments, using the Healthys back-end API.

This repository is an earlier version (Dart 2.7+, older dependencies) of the patient app,
whose most recent version is in `healthys_patients`. The sister apps
`helthysfront` (facilities and doctors) and `healthysvendeur` (resellers) complete the ecosystem.

## Main features

- **Account**: sign-up, login, password reset and password change, profile selection and
  switching, personal information.
- **App lock** (`flutter_app_lock`).
- **Patient dashboard**:
  - my digital health card (QR code) and card activation;
  - medical history, consultations, past history, medical record;
  - medical parameters and lifestyle;
  - vaccination record;
  - emergency information;
  - my health booklet;
  - my appointments (booking, viewing, editing);
  - Health News (health news).
- **Professional area** (depending on the profile): digital professional card, consultations, medical records,
  patient identification and card activation.
- **Directories**: healthcare staff, healthcare facilities (hospitals, centers, pharmacies, laboratories),
  insurers, brokers, banks, partners.
- **Subscriptions**: subscription sign-up (with or without a health ID number) and payment via an
  embedded web page.
- Self-screening, terms of use, privacy, PDF viewing, electronic signature.
- **Multilingual**: French, English and Spanish (`locale/i18n_*.json`), with a language selection screen.

## Tech stack

- **Language**: Dart (SDK `>=2.7.0 <3.0.0`)
- **Framework**: Flutter (Android, iOS)
- **Main dependencies**: `http`, `shared_preferences`, `flutter_app_lock`, `qr_flutter`,
  `image_picker`, `camera`, `flutter_image_compress`, `flutter_pdfview`, `webview_flutter`,
  `flutter_webview_plugin`, `syncfusion_flutter_signaturepad`, `cached_network_image`, `url_launcher`,
  `permission_handler`, `intl_phone_field`
- **Android application ID**: `cm.heatly`

## Project structure

```
lib/
├── main.dart        # Entry point: lock, language, splash page
├── config/          # Configuration (API URL), translations, singleton, image compression
├── pages/           # Screens (login, sign-up, home, digital card, appointments, vaccines…)
├── fragments/       # Dashboard sections (medical record, directories, insurance, emergency…)
├── forms/           # Forms (login, sign-up, appointment, vaccine, subscription sign-up…)
├── models/          # Data models (Compte, Profil, Medecin, Consultation, Vaccin, Abonnement…)
└── lock/            # App lock screen
locale/              # Translation files (fr, en, es)
img/                 # Images and icons
android/, ios/       # Native projects
```

## Installation and running

Prerequisites: a Flutter SDK compatible with Dart 2 (constraint `>=2.7.0 <3.0.0`), Android Studio / Xcode.
Since some dependencies are old, a Flutter version from the same era (Flutter 1.x / 2.x)
may be required.

```bash
flutter pub get
flutter run                 # run on a device or an emulator
flutter build apk           # build the Android APK
flutter build ios           # build the iOS app (on macOS)
```

## Configuration

The project does not include a `.env.example` file. The back-end server addresses (API, images, PDF)
are defined in `lib/config/Setting.dart` (`apiracine`, `serveurracine`, `serveurimage`,
`serveurimage1`, `serveurpdf`) and must be adapted to your environment before building.

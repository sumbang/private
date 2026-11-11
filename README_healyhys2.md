# Healthys 2 (Heatly – demo)

Flutter mobile app of the **Healthys** family, the initial / demo version of the Healthys app
(Android application ID `cm.heatly_demo`). It is intended for **patients** and, depending on the
logged-in user's profile, for **healthcare professionals**. It lets users keep their health information
online (digital card, medical record, vaccinations), browse directories and manage
appointments, using the Healthys back-end API.

Later versions of the patient app are in `healthys_patient` and then
`healthys_patients`; `helthysfront` (facilities and doctors) and `healthysvendeur` (resellers) complete
the ecosystem.

## Main features

- **Account**: sign-up, login, password reset and password change.
- **Profile-based dashboard**:
  - *Patient*: my digital health card, latest consultation, lifestyle, health insurance,
    appointment booking, doctor directory, vaccination record, health booklet;
  - *Professional*: my digital professional card, my consultations, medical records, my appointments,
    patient identification and card activation by **QR code scan** (`qrscan`).
- **Medical record**: consultations, past medical history, medical report.
- **Appointments**: booking, viewing and editing.
- **Directories**: doctors, hospitals, centers, pharmacies, laboratories, insurers, brokers, banks.
- **Subscriptions**: sign-up (with or without a health ID number).
- Emergency information, health news, terms of use.
- **Multilingual**: French, English and Spanish (`locale/i18n_*.json`).

## Tech stack

- **Language**: Dart (SDK `>=2.7.0 <3.0.0`)
- **Framework**: Flutter (Android, iOS)
- **Main dependencies**: `http`, `shared_preferences`, `qr_flutter`, `qrscan`, `image_picker`,
  `camera`, `webview_flutter`, `flutter_webview_plugin`, `cached_network_image`, `flutter_swiper`,
  `flutter_datetime_picker`, `intl_phone_field`
- **Android application ID**: `cm.heatly_demo`

## Project structure

```
lib/
├── main.dart        # App entry point
├── config/          # Configuration (API URL), translations, screen sizes
├── pages/           # Screens (login, sign-up, home, digital card, appointments, vaccines…)
├── fragments/       # Dashboard sections (medical record, directories, insurance, emergency…)
├── forms/           # Forms (login, sign-up, appointment, vaccine, subscription sign-up…)
└── models/          # Data models (Compte, Profil, Medecin, Consultation, Vaccin…)
locale/              # Translation files (fr, en, es)
img/                 # Images and icons
android/, ios/       # Native projects
```

Note: the repository also contains build artifacts (`build/`, `.dart_tool/`, `.packages`,
`.flutter-plugins*`) that can be regenerated and are not needed for development.

## Installation and running

Prerequisites: a Flutter SDK compatible with Dart 2 (constraint `>=2.7.0 <3.0.0`); since the dependencies
are old, a Flutter version from the same era (Flutter 1.x / 2.x) may be required.

```bash
flutter clean               # removes the committed build artifacts
flutter pub get
flutter run                 # run on a device or an emulator
flutter build apk           # build the Android APK
```

## Configuration

The project does not include a `.env.example` file. The back-end server addresses are defined in
`lib/config/Setting.dart` and must be adapted to your environment before building.

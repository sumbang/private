# Healthys Patients

Flutter mobile app of the **Healthys** family, intended for **patients**. It lets them keep their
health information online (digital health card, health booklet, medical history,
vaccination record), find healthcare professionals and facilities, and book appointments,
using the Healthys back-end API.

This is the most recent version of the patient app (see also `healthys_patient` and
`healyhys2`, earlier versions). The sister apps `helthysfront` (facilities and doctors) and
`healthysvendeur` (resellers) complete the ecosystem.

## Main features

- **Account**: sign-up, login, password reset and password change, management of
  multiple profiles linked to the account (profile switching), personal information.
- **App lock** (`flutter_app_lock`) after a period of inactivity.
- **Patient dashboard**:
  - my digital health card (with QR code) and card activation;
  - medical history, consultations, past medical history, medical record;
  - medical parameters and lifestyle;
  - vaccination record (adding vaccines);
  - emergency information;
  - my health booklet;
  - my appointments (booking, viewing, editing);
  - Health News (health news).
- **Professional area** (depending on the profile): digital professional card, consultations, medical records,
  appointments, patient identification and card activation.
- **Directories**: healthcare staff (doctors), healthcare facilities (hospitals, centers, pharmacies,
  laboratories), insurers, insurance brokers, banks, partners.
- **Subscriptions**: subscription sign-up (with or without a health ID number) and payment via an
  embedded web page.
- Self-screening, terms of use and privacy policy.
- PDF viewing, photo capture / gallery, image compression, electronic signature.
- **Multilingual**: French, English and Spanish (`locale/i18n_*.json`), with a language selection screen.

## Tech stack

- **Language**: Dart (SDK `>=2.18.4 <3.0.0`)
- **Framework**: Flutter (Android, iOS, web, Linux, macOS, Windows)
- **Main dependencies**: `http`, `shared_preferences`, `flutter_app_lock`, `qr_flutter`,
  `image_picker`, `camera`, `flutter_image_compress`, `flutter_pdfview`, `flutter_webview_plugin`,
  `webview_flutter_plus`, `syncfusion_flutter_signaturepad`, `cached_network_image`, `url_launcher`,
  `permission_handler`, `intl_phone_field`
- **App version**: 1.0.1+23 — Android application ID `cm.heatly`

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
android/, ios/, web/, linux/, macos/, windows/   # Platform projects
```

## Installation and running

Prerequisites: Flutter 3.x with Dart `>=2.18.4 <3.0.0`, Android Studio / Xcode.

```bash
flutter pub get
flutter run                 # run on a device or an emulator
flutter build apk           # build the Android APK
flutter build ios           # build the iOS app (on macOS)
```

To regenerate the app icons:

```bash
flutter pub run flutter_launcher_icons:main
```

## Configuration

The project does not include a `.env.example` file. The back-end server addresses (API, images, PDF)
are defined in `lib/config/Setting.dart` (`apiracine`, `serveurracine`, `serveurimage`,
`serveurimage1`, `serveurpdf`) and must be adapted to your environment before building.

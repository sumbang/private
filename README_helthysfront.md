# Healthys Front (Healthys Hôpital)

Flutter mobile app of the **Healthys** family, intended for **healthcare professionals and facilities**
(administrative staff, nurses, doctors). It lets them create and activate patient accounts,
manage appointments and consultations, and view patients' medical records, using the Healthys
back-end API.

It complements the other apps of the ecosystem:
- `healthys_patients` / `healthys_patient`: app for patients;
- `healthysvendeur`: app for resellers (registration of patients and companies).

## Main features

- **Login** with a username (or registration number) and password, password reset,
  profile selection ("Administration", "Nurse") and profile switching during a session.
- **App lock** (`flutter_app_lock`) after a period of inactivity in the background.
- **Two dashboards depending on the user's role**:
  - *Facility / administration area*: create a patient account, activate a patient account,
    new appointment, appointment management, company registration;
  - *Doctor area*: new consultation, consultation management, appointment management,
    patients' medical records.
- **Consultations**: creation, editing, details, closing a consultation, medical report,
  tracking of parameter changes.
- **Appointments**: creation, editing, listing and closing appointments.
- **Financial statements**: display of the commission and account balance, payment request
  (with or without the patient's signature, via a signature pad).
- Photo capture / image selection (camera, gallery), image compression, file upload,
  PDF viewing, QR code generation.
- **Multilingual**: French, English and Spanish (`locale/i18n_*.json`), with a language selection screen.

## Tech stack

- **Language**: Dart (SDK `>=2.7.0 <3.0.0`)
- **Framework**: Flutter (Android, iOS, web)
- **Main dependencies**: `http`, `dio`, `shared_preferences`, `flutter_app_lock`, `image_picker`,
  `camera`, `file_picker`, `flutter_image_compress`, `flutter_pdfview`, `qr_flutter`,
  `syncfusion_flutter_signaturepad`, `webview_flutter_plus`, `cached_network_image`, `fluttertoast`,
  `url_launcher`, `permission_handler`, `intl_phone_field`
- **Android application ID**: `com.novasos.healthys_hopital`

## Project structure

```
lib/
├── main.dart        # Entry point: lock, language, role-based redirection
├── config/          # Configuration (API URL), translations, singleton, screen sizes
├── page/            # Screens (login, home screens, consultations, appointments, patients, balances…)
├── form/            # Forms (login, patient, consultation, appointment, company, payment…)
├── fragment/        # Home, appointment and medical report fragments
├── model/           # Data models (Compte, Profil, Meeting, Docteur, Consultation…)
└── lock/            # App lock screen
locale/              # Translation files (fr, en, es)
img/                 # App images and icons
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

To regenerate the app icons:

```bash
flutter pub run flutter_launcher_icons:main
```

## Configuration

The project does not include a `.env.example` file. The back-end server addresses (API, images, PDF)
are defined in `lib/config/Setting.dart` (`apiracine`, `serveurracine`, `serveurimage`,
`serveurimage1`, `serveurpdf`) and must be adapted to your environment before building.

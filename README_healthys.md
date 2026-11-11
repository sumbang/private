# Health'ys – Mobile app (patients, doctors, hospitals)

Flutter app for the **Health'ys** healthcare platform. It lets patients store and share their
digital health booklet, lets doctors manage their consultations and patient follow-up, and lets
hospital staff (managers, nurses) handle reception, appointments and consultations within a facility.

The app consumes the Health'ys platform REST API (`lib/data/api` layer).

Current version: `2.1.3+49` (see `pubspec.yaml`).

## Main features

**Common**
- Onboarding, language selection (French, English, Spanish)
- Sign-up, login, account activation, password reset and password change
- Creation of a Patient or Doctor profile, selection of the active profile
- Terms of use and privacy policy
- PDF document and image viewing
- Push notifications (Firebase Cloud Messaging) and local notifications
- App update checks (`upgrader`)

**Patient**
- Digital health card with QR code
- Profile, past medical history, emergency contacts, medical record documents
- Consultation list, details and archive, consultation follow-up
- Booking and listing appointments (added to the phone's calendar)
- Directories: doctors, hospitals, pharmacies, laboratories; reviews / ratings
- Messaging with doctors
- Profile transfer
- Customer support via live chat (Tawk.to)

**Doctor**
- Dashboard, waiting room
- Reading the patient card by scanning its QR code
- Starting and conducting a consultation: past medical history, medical parameters,
  diagnoses, tests, medication prescriptions
- Access to the patient record, consultation import, archive
- Appointment management and follow-up, messaging with patients
- Profile update, professional card

**Hospital (manager / nurse)**
- Patient creation and subscription management
- Starting and following up consultations, waiting room
- Facility appointment management

## Tech stack

- **Flutter** / **Dart** (SDK constraint `>=2.19.6 <3.0.0`; CI uses Flutter 3.22.0)
- State management: `hooks_riverpod`, `flutter_hooks`; dependency injection: `get_it`
- Networking: `dio`, `http`
- Firebase: Core, Messaging, Analytics, Crashlytics, Performance
- Local storage: `shared_preferences`, `sqflite`
- Internationalization: `flutter_localizations` + ARB files (`lib/l10n`)
- Other: `qr_flutter`, `mobile_scanner`, `file_picker`, `image_picker`,
  `flutter_image_compress`, `flutter_cached_pdfview`, `flutter_signature_pad`,
  `add_2_calendar`, `flutter_local_notifications`, `flutter_tawkto`, `url_launcher`
- CI: GitHub Actions (`.github/workflows/code_quality.yml`) – `flutter pub get`,
  `flutter test --coverage`, coverage upload to Codecov

## Project structure

```
lib/
├── core/                 # Settings, translations, navigation, notifications, SQLite, utilities
├── data/
│   ├── api/              # REST API client (remote_api.dart)
│   ├── models/request/   # Request models
│   ├── models/response/  # Response models
│   └── repository/       # Repository implementations
├── domain/
│   ├── entities/         # Domain entities
│   ├── repository/       # Repository interfaces
│   └── usercase/         # Use cases (auth, account, data)
├── l10n/                 # ARB files and generated localizations (fr, en, es)
├── presentation/
│   ├── components/
│   │   ├── view_models/  # View models / Riverpod states
│   │   └── widgets/      # Reusable components
│   └── screens/
│       ├── common/       # Shared screens (profile creation, PDF, details…)
│       └── mobile/       # auth/, patient/, medecin/, hopital/, fragments/
├── firebase_options.dart
└── main.dart
assets/                   # Fonts
img/                      # App images and icons
locale/                   # Legacy JSON translation files (fr, en, es)
android/ ios/ web/ linux/ macos/ windows/   # Platform projects
test/                     # Tests
```

## Installation and running

Prerequisites: Flutter SDK installed (`flutter doctor`), an emulator or a connected device.

```bash
flutter pub get          # install dependencies
flutter gen-l10n         # (re)generate localizations if needed
flutter run              # run the app
flutter test             # run the tests
```

Building:

```bash
flutter build apk        # Android
flutter build ios        # iOS (on macOS)
flutter build web        # Web
```

App icons: `dart run flutter_launcher_icons`.

## Configuration

- No `.env.example` file is provided.
- The Firebase configuration is generated in `lib/firebase_options.dart` (FlutterFire);
  the native Firebase configuration files must be present in `android/` and `ios/`.
- The API base address is defined by the `url` constant in `lib/data/api/remote_api.dart`;
  the other app settings (colors, icons, utilities) are in `lib/core/setting.dart`.

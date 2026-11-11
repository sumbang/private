# Wouri TV (v4)

**Flutter** app for **Wouri TV**, "the crossroads of African cinemas": a video-on-demand
platform offering African films and series. This 4.x version consumes the
Wouri TV backend REST API and targets mobile (Android / iOS), tablet and web, with
dedicated screens for each form factor.

Intended audience: Wouri TV viewers.

## Main features

- **User account**: onboarding, sign-up, sign-in, account activation by code,
  password reset and change, profile, account deletion, help.
- **Catalog**: featured carousel, new releases, Wouri content, purchased films,
  liked films, films, series, store, search.
- **Film page**: content details, trailer, ratings and "likes", adding to
  "My List", sharing.
- **Video playback**: full-screen player (mobile and web), saving of playback
  progress, casting to a TV, screen kept awake during playback.
- **Downloads**: film downloads for offline viewing and an offline
  player.
- **Subscriptions and purchases**: subscription plans, payment initialization and
  ordering, history of subscriptions and purchases.
- **Viewing codes**: activation of a code that grants access to content.
- **TV connection**: viewing the TV linked to the account.
- Push **notifications** (Firebase Cloud Messaging) and notification center.
- Layouts adapted to mobile, tablet and web (portrait / landscape), bilingual
  **French / English** interface, app update prompts.

## Tech stack

- Flutter / Dart (SDK `>=2.19.6 <3.0.0`)
- State management: Riverpod (`hooks_riverpod`, `flutter_hooks`), GetX; `get_it` dependency injection
- Networking: `dio`
- Video: `video_player`, `flutter_cast_video`, `wakelock_plus`; local package
  `smooth_video_progress` (`dependencies/` folder)
- Local storage: Hive, sqflite, shared_preferences; downloads: `flutter_downloader`
- Encryption: `encrypt`, `crypto`
- Firebase: Core, Messaging, Analytics, Crashlytics, Performance
- Internationalization: `flutter_localizations`, ARB files (`lib/l10n`)
- CI: GitHub Actions (`flutter test --coverage`, quality analysis)

## Project structure

```
lib/
├── main.dart / main_web.dart   # mobile/tablet and web entry points
├── core/                       # settings, push notifications, SQLite, singletons, utilities
├── data/
│   ├── api/remote_api.dart     # Wouri TV REST API client
│   ├── models/                 # requests and responses
│   └── repository/             # repository implementations
├── domain/                     # entities, repository interfaces, use cases (auth, account, films, payment)
├── presentation/
│   ├── components/             # reusable widgets and view models
│   └── screens/                # mobile/, tablette/, web/ and common/ screens (players, payment…)
└── l10n/                       # translations (fr, en)
dependencies/smooth_video_progress/   # local Flutter package
assets/, img/                   # fonts and images
android/, ios/, web/, macos/, linux/, windows/
```

## Installation and running

Prerequisites: Flutter SDK compatible with Dart 2.19.6 (< 3.0), Android Studio and/or Xcode.

```bash
flutter pub get
flutter run                                   # mobile / tablet (lib/main.dart)
flutter run -d chrome -t lib/main_web.dart    # web version
```

Tests:

```bash
flutter test
```

## Configuration

No `.env.example` file is provided. The app relies on the Firebase configuration
of the platform projects and `lib/firebase_options.dart`, as well as on the
settings in `lib/core/setting.dart`.

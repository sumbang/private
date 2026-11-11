# Joodi — client app

**Flutter** app for **Joodi**, a platform for African radio dramas and podcasts.
It lets listeners discover the catalog, listen to content via streaming or
offline, and manage their account and subscriptions. The same codebase produces the mobile
app (Android / iOS) and a web version.

Intended audience: Joodi listeners.

## Main features

- **Home and discovery**: featured carousel, content sections, categories,
  search, podcast and episode detail pages, store (web).
- **Audio player**: streaming playback with a mini player and a full-screen player, background
  playback and control from system notifications.
- **Offline**: episode downloads and playback of downloaded content; local
  cache of API responses.
- **User account**: onboarding, sign-up, sign-in, activation by code,
  password reset and change, profile, account deletion, language
  selection.
- **Subscriptions and purchases**: browsing plans, payment initialization
  (opens the Joodi payment page in the browser), promo codes, history of
  subscriptions and purchases.
- **My List**, ratings and "likes" on content, sharing.
- Push **notifications** (Firebase Cloud Messaging) and notification center.
- Network connectivity detection and app update prompts.
- Bilingual **French / English** interface.

## Tech stack

- Flutter / Dart (SDK `>=3.1.5 <4.0.0`)
- State management: Riverpod (`hooks_riverpod`, `flutter_hooks`), GetX; `get_it` dependency injection
- Networking: `dio`, `http`
- Audio / video: `just_audio`, `just_audio_background`, `video_player`
- Local storage: Hive, sqflite, shared_preferences; downloads: `flutter_downloader`
- Firebase: Core, Messaging, Analytics, Crashlytics, Performance
- Internationalization: `flutter_localizations`, ARB files (`lib/l10n`)
- CI: GitHub Actions (`flutter test --coverage`, quality analysis)

## Project structure

```
lib/
├── main.dart / main_web.dart   # mobile and web entry points
├── core/                       # configuration, push notifications, connectivity, SQLite, singletons
├── audio/                      # audio player widgets and controls (mobile and web)
├── data/                       # API calls, request/response models, repository implementations
├── domain/                     # entities, repository interfaces, use cases (auth, account, content, payment)
├── presentation/
│   ├── components/             # reusable widgets and view models
│   └── screens/                # mobile/, web/ and common/ screens
└── l10n/                       # translations (fr, en)
locale/                         # JSON translation files
assets/, img/                   # fonts and images
android/, ios/, web/, macos/, linux/, windows/   # platform projects
```

## Installation and running

Prerequisites: Flutter SDK compatible with Dart 3.1.5+, Android Studio and/or Xcode.

```bash
flutter pub get
flutter run                          # mobile (entry point lib/main.dart)
flutter run -d chrome -t lib/main_web.dart   # web version
```

Generating Hive adapters when models change:

```bash
dart run build_runner build
```

Tests:

```bash
flutter test
```

## Configuration

No `.env.example` file is provided. The app relies on the Firebase configuration
of the platform projects (`google-services.json`, `GoogleService-Info.plist`,
`lib/firebase_options.dart`) and on the settings in `lib/core/setting.dart`.

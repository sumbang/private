# TSS Courses

Flutter e-learning app **"Tss Performance, A chacun son tableau de bord"**. It lets enrolled learners follow video courses (organized into chapters and lessons), browse free courses and download videos to watch offline. The app comes in mobile, tablet, desktop and web versions.

## Main features

- Onboarding and login by e-mail address or username, logout
- Session limited to one device: if a new device logs in with the same account, the current session is closed
- **My courses**: courses the user is enrolled in, with their chapters and lessons
- **Free courses**
- Online video playback (videos hosted on Vimeo) and progress tracking (start / end of each topic sent to the server)
- **Downloads**: download videos in a chosen format, queue management (pause, cancel, failure), offline playback, limit of 50 downloads
- **Settings**: account information, notifications, links (about, privacy policy, terms of use, subscription plans, contact), app version
- Caching of API responses (Hive) for degraded offline operation
- Push notifications (Firebase Cloud Messaging), Firebase Analytics, Crashlytics and Performance
- French and English interface; dedicated layouts for mobile, tablet, PC and web

## Tech stack

- **Flutter** (Dart SDK `>=3.4.4 <4.0.0`)
- State management: `hooks_riverpod` / `flutter_hooks`, `get`, `get_it` dependency injection
- Networking: `dio`
- Local storage: `hive` / `hive_flutter`, `sqflite`, `shared_preferences`, `path_provider`
- Video: `video_player`, `pod_player`, `flutter_cast_video`, `wakelock_plus`, local package `smooth_video_progress`
- Downloads: `flutter_downloader`, `flutter_download_manager`
- Firebase: core, messaging, analytics, crashlytics, performance
- Internationalization: `flutter_localizations`, `intl` (ARB files)
- Targets: Android, iOS, web, Windows, macOS, Linux

## Project structure

```
lib/
├── main.dart            # Mobile / tablet entry point
├── main_web.dart        # Web entry point
├── main_desktop.dart    # Desktop entry point
├── core/                # Settings, themes, navigation, Hive cache, SQLite database, downloads, Vimeo integration
├── data/
│   ├── api/             # REST API calls (remote_api.dart)
│   ├── models/          # Request / response models (login, account, course, chapter, lesson)
│   └── repositories/    # Data repository implementation
├── domain/              # Entities, repository interface, use cases
├── presentation/
│   ├── components/      # View models and widgets (course cards, chapters, lessons…)
│   └── screens/         # Per-platform screens: mobile/, tablette/, pc/, web/, commons/ (players, onboarding)
└── l10n/                # ARB translations (fr, en)
dependencies/smooth_video_progress/  # Local Flutter package
assets/, img/            # Fonts and images
```

## Installation and running

Prerequisites: Flutter installed, and a Firebase project configured for the target platforms.

```bash
flutter pub get

# Mobile / tablet
flutter run --dart-define=API_KEY_VIMEO=<your_key>
# Web
flutter run -d chrome -t lib/main_web.dart --dart-define=API_KEY_VIMEO=<your_key>
# Desktop
flutter run -d windows -t lib/main_desktop.dart --dart-define=API_KEY_VIMEO=<your_key>
```

Tests:

```bash
flutter test
```

## Configuration

There is no `.env.example` file. The following value is read at build time via `String.fromEnvironment`:

| Name | Purpose |
| --- | --- |
| `API_KEY_VIMEO` | Vimeo API access key used to fetch the videos |

The backend API URL is defined in `lib/data/api/remote_api.dart`.

# Wouri TV (v5)

**Flutter** app for **Wouri TV**, "the crossroads of African cinemas": a streaming platform
for African films, series and video content. This version (5.x) relies on
**Supabase** for authentication and data, and on a Supabase server function that
issues signed playback URLs for premium content.

Intended audience: Wouri TV viewers on mobile (Android / iOS) and web.

## Main features

- **Authentication** (Supabase Auth): email / password sign-up and sign-in,
  Google and Apple sign-in, password reset, onboarding.
- **Catalog**: latest films, most recommended films, most watched films, random
  selection, browsing by category and by channel, full list, search.
- **Film page**: details of a film and its content, user recommendations.
- **Video playback**: YouTube player for free content; for premium content,
  the purchase or rental is verified, then playback uses a signed URL.
- **My List**: adding films to and removing them from a personal list.
- **Profile**: account information, password change, sign-out.
- Push notifications (Firebase Cloud Messaging), analytics and crash reporting
  (Firebase Analytics, Crashlytics, Performance), app update prompts.
- Bilingual **French / English** interface; local cache of responses (Hive).

### Supabase function `generate-signed-url`

Edge Function (Deno / TypeScript) that, after verifying a shared secret, checks in the
database that the user has actually purchased the film (and that the rental has not expired), then generates
a signed CloudFront URL for playing the video file.

## Tech stack

- Flutter / Dart (SDK `^3.8.1`)
- Supabase (`supabase_flutter`): authentication, database, Edge Functions (Deno)
- State management: Riverpod (`hooks_riverpod`), GetX; `get_it` dependency injection
- Video: `youtube_player_flutter`, `video_player`, `chewie`
- Networking: `dio`; local storage: Hive, shared_preferences
- Firebase: Core, Messaging, Analytics, Crashlytics, Performance
- Internationalization: Flutter `l10n` code generation (fr, en)
- CI/CD: GitHub Actions (tests, APK/AAB and IPA builds, deployment to Google Play and TestFlight)

## Project structure

```
lib/
├── main.dart                 # entry point, Supabase and Firebase initialization
├── config/                   # theme, Riverpod providers, translations, settings, Hive cache
├── data/
│   ├── api/                  # Supabase services (auth, films, premium video)
│   ├── model/                # data models
│   └── repository/           # repository implementations
├── domain/                   # entities, repository interfaces, use cases
├── presentation/
│   ├── component/            # reusable widgets and view models
│   └── screen/               # auth/, home/ (home, search, list, profile), movie/ screens
└── l10n/                     # generated translations
supabase/functions/generate-signed-url/   # Edge Function that generates signed URLs
distribution/whatsnew/        # release notes for the stores
assets/                       # images and fonts
android/, ios/, web/, macos/, linux/, windows/
```

## Installation and running

Prerequisites: Flutter SDK compatible with Dart 3.8.1+, Android Studio and/or Xcode, a Supabase project.

```bash
flutter pub get
flutter run --dart-define=SUPABASE_URL=<url_du_projet> --dart-define=SUPABASE_KEY=<cle_publique>
```

Production build (examples):

```bash
flutter build apk --release --dart-define=SUPABASE_URL=... --dart-define=SUPABASE_KEY=...
flutter build ios --release --dart-define=SUPABASE_URL=... --dart-define=SUPABASE_KEY=...
```

Tests:

```bash
flutter test
```

Deploying the Edge Function (with the Supabase CLI):

```bash
supabase functions deploy generate-signed-url
```

## Configuration

No `.env.example` file is provided.

- App (passed via `--dart-define`): `SUPABASE_URL`, `SUPABASE_KEY`.
- `generate-signed-url` Edge Function (Supabase secrets): `SUPABASE_URL`,
  `SUPABASE_SERVICE_ROLE_KEY`, `FUNCTION_SECRET`, `CLOUDFRONT_DOMAIN`,
  `CLOUDFRONT_KEY_PAIR_ID`, `CLOUDFRONT_PRIVATE_KEY`.
- Firebase: platform configuration files and `lib/firebase_options.dart`.

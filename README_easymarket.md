# Easy Market – mobile app

Flutter mobile app for the **Easy Market** e-commerce platform, aimed at Africans in the diaspora: it lets them buy goods and services and have them delivered directly in their home country. The app consumes the REST API of a WordPress / WooCommerce store.

## Main features

- Splash screen and onboarding, offline mode (dedicated dashboard)
- Catalog: categories, featured products, new arrivals, best sellers, promotions, grouped products, search
- Detailed product page
- Cart (add, update, remove, clear) synchronized with the server
- Promo codes (coupons) and shipping fees
- Placing and tracking orders (order list and details)
- Card payment via Stripe, payment success / failure screens
- Customer account: sign-up, login (including Google Sign-In and Sign in with Apple), profile, password change, account deletion
- Currency (USD / EUR) and language (French / English) selection
- Push notifications (Firebase Cloud Messaging) and local notifications
- Monitoring with Firebase Analytics, Crashlytics and Performance
- App update prompt (upgrader), sharing, deep links

## Tech stack

- **Flutter** (Dart SDK `>=3.13.0 <4.0.0`, Flutter 3.47.x in CI)
- State management: `hooks_riverpod` / `flutter_hooks`, `get`, `get_it` dependency injection
- Networking: `dio`, `http`
- Local storage: `hive_ce`, `shared_preferences`
- Payment: `flutter_stripe`
- Firebase: core, messaging, analytics, crashlytics, performance
- Third-party authentication: `google_sign_in`, `sign_in_with_apple`
- Internationalization: `flutter_localizations`, `intl` (ARB files)
- Backend consumed: WordPress / WooCommerce (REST API)
- Targets: Android, iOS (web, Linux, macOS and Windows folders are also present)

## Project structure

```
lib/
├── core/            # Cross-cutting services: push notifications, cart sync, language, currency, navigation, settings
├── data/
│   ├── api/         # REST API calls (remote_api.dart) and authentication (auth_service.dart)
│   ├── models/      # Request and response models
│   └── repositories/# Data repository implementation
├── domain/          # Entities, repository interface, use cases (clean architecture)
├── presentation/
│   ├── components/  # View models and reusable widgets
│   └── screens/     # Screens (home, products, cart, orders, payment, profile, login…)
├── l10n/            # ARB translations (fr, en) and generated classes
└── main.dart
assets/, img/        # Fonts and images
locale/              # JSON translation files
test/                # Unit / integration tests (cart synchronization, widget)
.github/workflows/   # Build and store deployment pipeline
.ai/                 # Context, architecture and coding-rule notes
```

## Installation and running

Prerequisites: Flutter installed, and a Firebase project configured for the target platforms.

```bash
flutter pub get
# Generate Hive adapters if needed
dart run build_runner build --delete-conflicting-outputs
# Run (configuration values are passed at build time)
flutter run --dart-define-from-file=dart-defines.json
```

Tests:

```bash
flutter test
```

Release builds:

```bash
flutter build apk --release --dart-define-from-file=dart-defines.json
flutter build appbundle --release --dart-define-from-file=dart-defines.json
flutter build ios --release --dart-define-from-file=dart-defines.json
```

Automated deployment to Google Play (internal track) and TestFlight is described in [DEPLOYMENT.md](./DEPLOYMENT.md).

## Configuration

The following values are read at build time via `String.fromEnvironment` (provide them in a file passed with `--dart-define-from-file`, and do not commit it):

| Name | Purpose |
| --- | --- |
| `WP_TOKEN` | Access token for the WordPress / WooCommerce API |
| `STRIPE_KEY` | Stripe publishable key |
| `GOOGLE_CLIENTID` | Google Sign-In client ID (server) |
| `GOOGLEIOS_CLIENTID` | Google Sign-In client ID (iOS) |

Since these values are embedded in the app binary, never use privileged credentials or server-side private keys.

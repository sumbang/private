# Klooztumi

Flutter mobile app (Android / iOS) that connects **local shops** with their **customers** in the form of a local social network. Merchants showcase their shop and products and publish news; customers discover nearby shops, follow them and interact with their posts.

The app is a client of a remote REST API (authentication, shops, products, posts, users).

## Main features

- **Onboarding** and splash screen.
- **Authentication**: login, token refresh, forgotten password, password change.
- **Two account types** with dedicated flows:
  - **Customer**: sign-up, news feed, list of followed shops, contacts, account management.
  - **Merchant**: sign-up, shop creation (categories, tags, address, GPS coordinates, opening hours, payment methods, website, banner and profile picture), product catalog management (add / edit), subscription / end of subscription.
- **Posts** with images: creation, details, comments, like / unlike, sharing, deletion.
- **Follows**: follow / unfollow a shop, favorites, list of a user's followers and followed accounts.
- **Search** for shops or people, taking the user's location into account.
- **Geolocation** (current position via `geolocator`) and opening directions to a shop in the maps app (`maps_launcher`).
- Personal **QR code** (generated with `qr_flutter`, scanned with `qrscan`).
- Image selection / compression, PDF viewing, file picking.
- Settings: notifications, display theme, privacy policy, logout.
- French-language interface (translation files `locale/i18n_fr.json` and `locale/i18n_en.json`).

## Tech stack

- **Flutter** / **Dart** (SDK `>=2.7.0 <3.0.0`, pre-null-safety code)
- Main dependencies: `http`, `shared_preferences`, `cached_network_image`, `image_picker`, `file_picker`, `flutter_image_compress`, `geolocator`, `flutter_map`, `maps_launcher`, `qr_flutter`, `qrscan`, `flutter_slidable`, `carousel_slider`, `flutter_pdfview`, `permission_handler`, `url_launcher`, `intl_phone_field`, `fluttertoast`, `font_awesome_flutter`
- App icons generated with `flutter_launcher_icons`
- Android application ID: `com.apps.klooztumi` (minSdk 21)

## Project structure

```
lib/
├── main.dart          # Application entry point
├── page/              # Screens (login, customer/merchant sign-up, home, shop details,
│                      #   product add/edit, post, search, settings, subscription…)
├── fragment/          # Tabs of the customer and merchant home screens (home, shop,
│                      #   products, contacts, account, settings)
├── model/             # Data models (User, Seller, Produit, Post, Comment, Tag,
│                      #   CategorieShop, HeureShop, LocationShop, PaymentMethod, Souscription…)
├── block/             # Reusable widgets (post preview, image, empty state)
└── config/            # Configuration (API URL in Setting.dart), shared widgets, utilities
assets/                # Fonts (Candara, FontAwesome)
img/                   # Images, logos and icons
locale/                # JSON translation files (fr, en)
android/, ios/, web/   # Native / platform projects
```

## Installation and running

Prerequisites: a Flutter SDK compatible with Dart 2.x (a version earlier than Dart 3) and an Android / iOS emulator or device.

```bash
flutter pub get
flutter run
```

Generate the app icons:

```bash
flutter pub run flutter_launcher_icons:main
```

Build a production release:

```bash
flutter build apk     # Android
flutter build ios     # iOS
```

## Configuration

No `.env` file is used. The REST API base URL is defined in `lib/config/Setting.dart` (constant `Setting.apiracine`); adjust it to your environment before running the app.

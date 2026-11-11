# Easy Market – application mobile

Application mobile Flutter de la plateforme e-commerce **Easy Market**, destinée aux Africains de la diaspora : elle leur permet d'acheter des biens et services et de les faire livrer directement dans leur pays d'origine. L'application consomme l'API REST d'une boutique WordPress / WooCommerce.

## Fonctionnalités principales

- Écran d'accueil et onboarding, mode hors ligne (tableau de bord dédié)
- Catalogue : catégories, produits mis en avant, nouveautés, meilleures ventes, promotions, produits groupés, recherche
- Fiche produit détaillée
- Panier (ajout, mise à jour, suppression, vidage) synchronisé avec le serveur
- Codes promo (coupons) et frais de livraison
- Passage et suivi de commandes (liste et détail des commandes)
- Paiement par carte via Stripe, écrans de succès / échec de paiement
- Compte client : inscription, connexion (y compris Google Sign-In et Sign in with Apple), profil, changement de mot de passe, suppression de compte
- Choix de la devise (USD / EUR) et de la langue (français / anglais)
- Notifications push (Firebase Cloud Messaging) et notifications locales
- Suivi via Firebase Analytics, Crashlytics et Performance
- Invitation à mettre à jour l'application (upgrader), partage, liens profonds

## Stack technique

- **Flutter** (SDK Dart `>=3.13.0 <4.0.0`, Flutter 3.47.x en CI)
- Gestion d'état : `hooks_riverpod` / `flutter_hooks`, `get`, injection `get_it`
- Réseau : `dio`, `http`
- Stockage local : `hive_ce`, `shared_preferences`
- Paiement : `flutter_stripe`
- Firebase : core, messaging, analytics, crashlytics, performance
- Authentification tierce : `google_sign_in`, `sign_in_with_apple`
- Internationalisation : `flutter_localizations`, `intl` (fichiers ARB)
- Backend consommé : WordPress / WooCommerce (API REST)
- Cibles : Android, iOS (dossiers web, Linux, macOS, Windows également présents)

## Structure du projet

```
lib/
├── core/            # Services transverses : notifications push, synchro du panier, langue, devise, navigation, réglages
├── data/
│   ├── api/         # Appels à l'API REST (remote_api.dart) et authentification (auth_service.dart)
│   ├── models/      # Modèles de requêtes et de réponses
│   └── repositories/# Implémentation du dépôt de données
├── domain/          # Entités, interface du dépôt, cas d'utilisation (clean architecture)
├── presentation/
│   ├── components/  # View models et widgets réutilisables
│   └── screens/     # Écrans (accueil, produits, panier, commandes, paiement, profil, connexion…)
├── l10n/            # Traductions ARB (fr, en) et classes générées
└── main.dart
assets/, img/        # Polices et images
locale/              # Fichiers de traduction JSON
test/                # Tests unitaires / d'intégration (synchronisation du panier, widget)
.github/workflows/   # Pipeline de build et de déploiement vers les stores
.ai/                 # Notes de contexte, architecture et règles de code
```

## Installation et lancement

Prérequis : Flutter installé, ainsi qu'un projet Firebase configuré pour les plateformes ciblées.

```bash
flutter pub get
# Génération éventuelle des adaptateurs Hive
dart run build_runner build --delete-conflicting-outputs
# Lancement (les valeurs de configuration sont passées à la compilation)
flutter run --dart-define-from-file=dart-defines.json
```

Tests :

```bash
flutter test
```

Builds de release :

```bash
flutter build apk --release --dart-define-from-file=dart-defines.json
flutter build appbundle --release --dart-define-from-file=dart-defines.json
flutter build ios --release --dart-define-from-file=dart-defines.json
```

Le déploiement automatisé vers Google Play (piste interne) et TestFlight est décrit dans [DEPLOYMENT.md](./DEPLOYMENT.md).

## Configuration

Les valeurs suivantes sont lues à la compilation via `String.fromEnvironment` (à fournir dans un fichier passé avec `--dart-define-from-file`, à ne pas versionner) :

| Nom | Rôle |
| --- | --- |
| `WP_TOKEN` | Jeton d'accès à l'API WordPress / WooCommerce |
| `STRIPE_KEY` | Clé publique Stripe |
| `GOOGLE_CLIENTID` | Client ID Google Sign-In (serveur) |
| `GOOGLEIOS_CLIENTID` | Client ID Google Sign-In (iOS) |

Ces valeurs étant intégrées au binaire de l'application, n'utilisez jamais d'identifiants privilégiés ou de clés privées côté serveur.

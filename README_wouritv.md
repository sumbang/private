# Wouri TV (v5)

Application **Flutter** de **Wouri TV**, « le carrefour des cinémas d'Afrique » : une plateforme
de diffusion de films, séries et contenus vidéo africains. Cette version (5.x) s'appuie sur
**Supabase** pour l'authentification et les données, et sur une fonction serveur Supabase qui
délivre des liens de lecture signés pour les contenus premium.

Public visé : les spectateurs de Wouri TV sur mobile (Android / iOS) et web.

## Fonctionnalités principales

- **Authentification** (Supabase Auth) : inscription et connexion par e-mail / mot de passe,
  connexion Google et Apple, réinitialisation du mot de passe, onboarding.
- **Catalogue** : derniers films, films les plus recommandés, les plus regardés, sélection
  aléatoire, navigation par catégorie et par chaîne, liste complète, recherche.
- **Fiche film** : détail d'un film et de ses contenus, recommandation par l'utilisateur.
- **Lecture vidéo** : lecteur YouTube pour les contenus gratuits ; pour les contenus premium,
  vérification de l'achat ou de la location puis lecture via une URL signée.
- **Ma liste** : ajout et retrait de films dans une liste personnelle.
- **Profil** : informations du compte, changement de mot de passe, déconnexion.
- Notifications push (Firebase Cloud Messaging), statistiques et rapports de plantage
  (Firebase Analytics, Crashlytics, Performance), invitation aux mises à jour.
- Interface bilingue **français / anglais** ; cache local des réponses (Hive).

### Fonction Supabase `generate-signed-url`

Fonction Edge (Deno / TypeScript) qui, après vérification d'un secret partagé, contrôle dans la
base que l'utilisateur a bien acheté le film (et que la location n'a pas expiré), puis génère
une URL CloudFront signée pour la lecture du fichier vidéo.

## Stack technique

- Flutter / Dart (SDK `^3.8.1`)
- Supabase (`supabase_flutter`) : authentification, base de données, fonctions Edge (Deno)
- Gestion d'état : Riverpod (`hooks_riverpod`), GetX ; injection `get_it`
- Vidéo : `youtube_player_flutter`, `video_player`, `chewie`
- Réseau : `dio` ; stockage local : Hive, shared_preferences
- Firebase : Core, Messaging, Analytics, Crashlytics, Performance
- Internationalisation : génération Flutter `l10n` (fr, en)
- CI/CD : GitHub Actions (tests, build APK/AAB et IPA, déploiement Google Play et TestFlight)

## Structure du projet

```
lib/
├── main.dart                 # point d'entrée, initialisation Supabase et Firebase
├── config/                   # thème, providers Riverpod, traductions, paramètres, cache Hive
├── data/
│   ├── api/                  # services Supabase (auth, films, vidéo premium)
│   ├── model/                # modèles de données
│   └── repository/           # implémentations des dépôts
├── domain/                   # entités, interfaces des dépôts, cas d'usage
├── presentation/
│   ├── component/            # widgets et view models réutilisables
│   └── screen/               # écrans auth/, home/ (accueil, recherche, liste, profil), movie/
└── l10n/                     # traductions générées
supabase/functions/generate-signed-url/   # fonction Edge de génération d'URL signées
distribution/whatsnew/        # notes de version pour les stores
assets/                       # images et polices
android/, ios/, web/, macos/, linux/, windows/
```

## Installation et lancement

Prérequis : Flutter SDK compatible Dart 3.8.1+, Android Studio et/ou Xcode, un projet Supabase.

```bash
flutter pub get
flutter run --dart-define=SUPABASE_URL=<url_du_projet> --dart-define=SUPABASE_KEY=<cle_publique>
```

Build de production (exemples) :

```bash
flutter build apk --release --dart-define=SUPABASE_URL=... --dart-define=SUPABASE_KEY=...
flutter build ios --release --dart-define=SUPABASE_URL=... --dart-define=SUPABASE_KEY=...
```

Tests :

```bash
flutter test
```

Déploiement de la fonction Edge (avec la CLI Supabase) :

```bash
supabase functions deploy generate-signed-url
```

## Configuration

Aucun fichier `.env.example` n'est fourni.

- Application (passées par `--dart-define`) : `SUPABASE_URL`, `SUPABASE_KEY`.
- Fonction Edge `generate-signed-url` (secrets Supabase) : `SUPABASE_URL`,
  `SUPABASE_SERVICE_ROLE_KEY`, `FUNCTION_SECRET`, `CLOUDFRONT_DOMAIN`,
  `CLOUDFRONT_KEY_PAIR_ID`, `CLOUDFRONT_PRIVATE_KEY`.
- Firebase : fichiers de configuration des plateformes et `lib/firebase_options.dart`.

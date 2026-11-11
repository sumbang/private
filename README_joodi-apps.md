# Joodi — application cliente

Application **Flutter** de **Joodi**, plateforme de films radiophoniques et de podcasts africains.
Elle permet aux auditeurs de découvrir le catalogue, d'écouter les contenus en streaming ou
hors ligne, et de gérer leur compte et leurs abonnements. Le même code produit l'application
mobile (Android / iOS) et une version web.

Public visé : les auditeurs de Joodi.

## Fonctionnalités principales

- **Accueil et découverte** : carrousel de mise en avant, rubriques de contenus, catégories,
  recherche, fiche détaillée d'un podcast et de ses épisodes, boutique (web).
- **Lecteur audio** : lecture en streaming avec mini-lecteur et lecteur plein écran, lecture
  en arrière-plan et contrôle depuis les notifications système.
- **Hors ligne** : téléchargement des épisodes et lecture des contenus téléchargés ; cache
  local des réponses de l'API.
- **Compte utilisateur** : onboarding, inscription, connexion, activation par code,
  réinitialisation et changement de mot de passe, profil, suppression du compte, choix de
  la langue.
- **Abonnements et achats** : consultation des formules, initialisation d'un paiement
  (ouverture de la page de paiement Joodi dans le navigateur), codes promo, historique des
  abonnements et des achats.
- **Ma liste**, notation et « j'aime » des contenus, partage.
- **Notifications** push (Firebase Cloud Messaging) et centre de notifications.
- Détection de la connectivité réseau et invitation aux mises à jour de l'application.
- Interface bilingue **français / anglais**.

## Stack technique

- Flutter / Dart (SDK `>=3.1.5 <4.0.0`)
- Gestion d'état : Riverpod (`hooks_riverpod`, `flutter_hooks`), GetX ; injection `get_it`
- Réseau : `dio`, `http`
- Audio / vidéo : `just_audio`, `just_audio_background`, `video_player`
- Stockage local : Hive, sqflite, shared_preferences ; téléchargements : `flutter_downloader`
- Firebase : Core, Messaging, Analytics, Crashlytics, Performance
- Internationalisation : `flutter_localizations`, fichiers ARB (`lib/l10n`)
- CI : GitHub Actions (`flutter test --coverage`, analyse de qualité)

## Structure du projet

```
lib/
├── main.dart / main_web.dart   # points d'entrée mobile et web
├── core/                       # configuration, notifications push, connectivité, SQLite, singletons
├── audio/                      # widgets et contrôles du lecteur audio (mobile et web)
├── data/                       # appels API, modèles de requêtes/réponses, implémentations des dépôts
├── domain/                     # entités, interfaces des dépôts, cas d'usage (auth, compte, contenus, paiement)
├── presentation/
│   ├── components/             # widgets et view models réutilisables
│   └── screens/                # écrans mobile/, web/ et common/
└── l10n/                       # traductions (fr, en)
locale/                         # fichiers de traduction JSON
assets/, img/                   # polices et images
android/, ios/, web/, macos/, linux/, windows/   # projets plateformes
```

## Installation et lancement

Prérequis : Flutter SDK compatible Dart 3.1.5+, Android Studio et/ou Xcode.

```bash
flutter pub get
flutter run                          # mobile (point d'entrée lib/main.dart)
flutter run -d chrome -t lib/main_web.dart   # version web
```

Génération des adaptateurs Hive si les modèles changent :

```bash
dart run build_runner build
```

Tests :

```bash
flutter test
```

## Configuration

Aucun fichier `.env.example` n'est fourni. L'application s'appuie sur la configuration
Firebase des projets plateformes (`google-services.json`, `GoogleService-Info.plist`,
`lib/firebase_options.dart`) et sur les paramètres de `lib/core/setting.dart`.

# Wouri TV (v4)

Application **Flutter** de **Wouri TV**, « le carrefour des cinémas d'Afrique » : une plateforme
de vidéo à la demande proposant films et séries africains. Cette version 4.x consomme l'API
REST du backend Wouri TV et cible le mobile (Android / iOS), la tablette et le web, avec des
écrans dédiés à chaque format.

Public visé : les spectateurs de Wouri TV.

## Fonctionnalités principales

- **Compte utilisateur** : onboarding, inscription, connexion, activation du compte par code,
  réinitialisation et changement de mot de passe, profil, suppression du compte, aide.
- **Catalogue** : carrousel de mise en avant, nouveautés, contenus Wouri, films achetés,
  films aimés, films, séries, boutique, recherche.
- **Fiche film** : détail du contenu, bande-annonce, notation et « j'aime », ajout à
  « Ma liste », partage.
- **Lecture vidéo** : lecteur plein écran (mobile et web), enregistrement de la progression
  de lecture, diffusion vers un téléviseur (cast), écran maintenu allumé pendant la lecture.
- **Téléchargements** : téléchargement des films pour un visionnage hors ligne et lecteur
  hors ligne.
- **Abonnements et achats** : formules d'abonnement, initialisation d'un paiement et
  commande, historique des abonnements et des achats.
- **Codes de visionnage** : activation d'un code donnant accès à des contenus.
- **Connexion TV** : consultation du téléviseur associé au compte.
- **Notifications** push (Firebase Cloud Messaging) et centre de notifications.
- Mises en page adaptées mobile, tablette et web (portrait / paysage), interface bilingue
  **français / anglais**, invitation aux mises à jour de l'application.

## Stack technique

- Flutter / Dart (SDK `>=2.19.6 <3.0.0`)
- Gestion d'état : Riverpod (`hooks_riverpod`, `flutter_hooks`), GetX ; injection `get_it`
- Réseau : `dio`
- Vidéo : `video_player`, `flutter_cast_video`, `wakelock_plus` ; paquet local
  `smooth_video_progress` (dossier `dependencies/`)
- Stockage local : Hive, sqflite, shared_preferences ; téléchargements : `flutter_downloader`
- Chiffrement : `encrypt`, `crypto`
- Firebase : Core, Messaging, Analytics, Crashlytics, Performance
- Internationalisation : `flutter_localizations`, fichiers ARB (`lib/l10n`)
- CI : GitHub Actions (`flutter test --coverage`, analyse de qualité)

## Structure du projet

```
lib/
├── main.dart / main_web.dart   # points d'entrée mobile/tablette et web
├── core/                       # paramètres, notifications push, SQLite, singletons, utilitaires
├── data/
│   ├── api/remote_api.dart     # client de l'API REST Wouri TV
│   ├── models/                 # requêtes et réponses
│   └── repository/             # implémentations des dépôts
├── domain/                     # entités, interfaces des dépôts, cas d'usage (auth, compte, films, paiement)
├── presentation/
│   ├── components/             # widgets et view models réutilisables
│   └── screens/                # écrans mobile/, tablette/, web/ et common/ (lecteurs, paiement…)
└── l10n/                       # traductions (fr, en)
dependencies/smooth_video_progress/   # paquet Flutter local
assets/, img/                   # polices et images
android/, ios/, web/, macos/, linux/, windows/
```

## Installation et lancement

Prérequis : Flutter SDK compatible avec Dart 2.19.6 (< 3.0), Android Studio et/ou Xcode.

```bash
flutter pub get
flutter run                                   # mobile / tablette (lib/main.dart)
flutter run -d chrome -t lib/main_web.dart    # version web
```

Tests :

```bash
flutter test
```

## Configuration

Aucun fichier `.env.example` n'est fourni. L'application s'appuie sur la configuration
Firebase des projets plateformes et de `lib/firebase_options.dart`, ainsi que sur les
paramètres de `lib/core/setting.dart`.

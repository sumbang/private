# TSS Courses

Application Flutter de formation en ligne **« Tss Performance, A chacun son tableau de bord »**. Elle permet aux apprenants inscrits de suivre des formations vidéo (organisées en chapitres et leçons), de consulter les cours gratuits et de télécharger les vidéos pour les regarder hors ligne. L'application est déclinée pour mobile, tablette, ordinateur (desktop) et web.

## Fonctionnalités principales

- Onboarding et connexion par adresse e-mail ou identifiant, déconnexion
- Session limitée à un appareil : si un nouvel appareil se connecte avec le même compte, la session courante est fermée
- **Mes cours** : formations auxquelles l'utilisateur est inscrit, avec leurs chapitres et leçons
- **Cours gratuits**
- Lecture vidéo en ligne (vidéos hébergées sur Vimeo) et suivi de progression (début / fin de chaque sujet envoyés au serveur)
- **Téléchargements** : téléchargement des vidéos dans un format choisi, gestion de la file (pause, annulation, échec), lecture hors ligne, limite de 50 téléchargements
- **Paramètres** : informations du compte, notifications, liens (à propos, politique de confidentialité, conditions d'utilisation, offres d'abonnement, contact), version de l'application
- Mise en cache des réponses de l'API (Hive) pour un fonctionnement dégradé hors connexion
- Notifications push (Firebase Cloud Messaging), Firebase Analytics, Crashlytics et Performance
- Interface en français et en anglais ; mises en page dédiées mobile, tablette, PC et web

## Stack technique

- **Flutter** (SDK Dart `>=3.4.4 <4.0.0`)
- Gestion d'état : `hooks_riverpod` / `flutter_hooks`, `get`, injection `get_it`
- Réseau : `dio`
- Stockage local : `hive` / `hive_flutter`, `sqflite`, `shared_preferences`, `path_provider`
- Vidéo : `video_player`, `pod_player`, `flutter_cast_video`, `wakelock_plus`, paquet local `smooth_video_progress`
- Téléchargements : `flutter_downloader`, `flutter_download_manager`
- Firebase : core, messaging, analytics, crashlytics, performance
- Internationalisation : `flutter_localizations`, `intl` (fichiers ARB)
- Cibles : Android, iOS, web, Windows, macOS, Linux

## Structure du projet

```
lib/
├── main.dart            # Point d'entrée mobile / tablette
├── main_web.dart        # Point d'entrée web
├── main_desktop.dart    # Point d'entrée desktop
├── core/                # Réglages, thèmes, navigation, cache Hive, base SQLite, téléchargements, intégration Vimeo
├── data/
│   ├── api/             # Appels à l'API REST (remote_api.dart)
│   ├── models/          # Modèles de requêtes / réponses (connexion, compte, formation, chapitre, leçon)
│   └── repositories/    # Implémentation du dépôt de données
├── domain/              # Entités, interface du dépôt, cas d'utilisation
├── presentation/
│   ├── components/      # View models et widgets (cartes de cours, chapitres, leçons…)
│   └── screens/         # Écrans par plateforme : mobile/, tablette/, pc/, web/, commons/ (lecteurs, onboarding)
└── l10n/                # Traductions ARB (fr, en)
dependencies/smooth_video_progress/  # Paquet Flutter local
assets/, img/            # Polices et images
```

## Installation et lancement

Prérequis : Flutter installé, ainsi qu'un projet Firebase configuré pour les plateformes ciblées.

```bash
flutter pub get

# Mobile / tablette
flutter run --dart-define=API_KEY_VIMEO=<votre_cle>
# Web
flutter run -d chrome -t lib/main_web.dart --dart-define=API_KEY_VIMEO=<votre_cle>
# Desktop
flutter run -d windows -t lib/main_desktop.dart --dart-define=API_KEY_VIMEO=<votre_cle>
```

Tests :

```bash
flutter test
```

## Configuration

Il n'y a pas de fichier `.env.example`. La valeur suivante est lue à la compilation via `String.fromEnvironment` :

| Nom | Rôle |
| --- | --- |
| `API_KEY_VIMEO` | Clé d'accès à l'API Vimeo pour récupérer les vidéos |

L'URL de l'API backend est définie dans `lib/data/api/remote_api.dart`.

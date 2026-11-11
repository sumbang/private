# Healthys Vendeur

Application mobile Flutter de la famille **Healthys**, destinée aux **revendeurs / agents commerciaux**
du réseau Healthys. Elle leur permet d'enregistrer de nouveaux patients, d'activer leurs comptes
(abonnements) et d'enregistrer des entreprises partenaires, en s'appuyant sur l'API back-end Healthys.

Elle complète les autres applications de l'écosystème : `healthys_patients` / `healthys_patient`
(patients) et `helthysfront` (structures et médecins).

## Fonctionnalités principales

- **Connexion** du revendeur, réinitialisation et changement du mot de passe, page « Mon compte ».
- **Enregistrer un patient** : saisie de l'identité, des coordonnées (téléphone, e-mail, adresse, ville),
  de la profession, de la pièce d'identité, etc., avec prise de photo ; écran de confirmation
  « Nouveau patient ».
- **Activer un compte** : saisie ou **scan du QR code** du matricule patient, choix de l'abonnement et
  du mode de paiement, code revendeur et référence de paiement.
- **Enregistrer une entreprise** : raison sociale, activité, responsable, coordonnées, ville et district.
- **Multilingue** : fichiers de traduction français, anglais et espagnol (`locale/i18n_*.json`).

## Stack technique

- **Langage** : Dart (SDK `>=2.7.0 <3.0.0`)
- **Framework** : Flutter (Android, iOS, web)
- **Principales dépendances** : `http`, `shared_preferences`, `qrscan`, `qr_flutter`, `image_picker`,
  `camera`, `webview_flutter`, `flutter_webview_plugin`, `cached_network_image`, `fluttertoast`,
  `flutter_datetime_picker`, `intl_phone_field`, `flutter_pdfview`
- **Identifiant Android** : `cm.healthys_vendeur`

## Structure du projet

```
lib/
├── main.dart        # Point d'entrée de l'application
├── config/          # Configuration (URL de l'API), traductions, compression d'images
├── page/            # Écrans (connexion, accueil, nouveau patient, activation, entreprise, compte…)
├── form/            # Formulaires (connexion, patient, activation, entreprise, mot de passe)
├── fragment/        # Fragment d'accueil
└── model/           # Modèles de données (Compte, Profil, Menu…)
locale/              # Fichiers de traduction (fr, en, es)
img/                 # Images et icônes
android/, ios/, web/ # Projets natifs et cible web
```

## Installation et lancement

Prérequis : un SDK Flutter compatible avec Dart 2 (contrainte `>=2.7.0 <3.0.0`), Android Studio / Xcode.

```bash
flutter pub get
flutter run                 # lancer sur un appareil ou un émulateur
flutter build apk           # générer l'APK Android
flutter build ios           # générer l'application iOS (sur macOS)
```

## Configuration

Le projet ne contient pas de fichier `.env.example`. Les adresses du serveur back-end sont définies dans
`lib/config/Setting.dart` et doivent être adaptées à votre environnement avant la compilation.

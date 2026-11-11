# Healthys Patient (Heatly)

Application mobile Flutter de la famille **Healthys**, destinée aux **patients**. Elle leur permet de
conserver leurs informations de santé en ligne (carte digitale santé, carnet de santé, historique médical,
carnet de vaccination), de consulter des annuaires de professionnels et structures de santé et de gérer
leurs rendez-vous, en s'appuyant sur l'API back-end Healthys.

Ce dépôt correspond à une version antérieure (Dart 2.7+, dépendances plus anciennes) de l'application
patient, dont la version la plus récente se trouve dans `healthys_patients`. Les applications sœurs
`helthysfront` (structures et médecins) et `healthysvendeur` (revendeurs) complètent l'écosystème.

## Fonctionnalités principales

- **Compte** : inscription, connexion, réinitialisation et changement du mot de passe, choix et
  changement de profil, informations personnelles.
- **Verrouillage de l'application** (`flutter_app_lock`).
- **Tableau de bord patient** :
  - ma carte digitale santé (QR code) et activation de carte ;
  - historique médical, consultations, antécédents, dossier médical ;
  - paramètres médicaux et mode de vie ;
  - carnet de vaccination ;
  - informations de première urgence ;
  - mon carnet de santé ;
  - mes rendez-vous (prise, consultation, modification) ;
  - Health News (actualités santé).
- **Espace professionnel** (selon le profil) : carte digitale pro, consultations, dossiers médicaux,
  identification d'un patient et activation de carte.
- **Annuaires** : personnels de santé, structures de santé (hôpitaux, centres, pharmacies, laboratoires),
  assurances, courtiers, banques, partenaires.
- **Abonnements** : souscription à un abonnement (avec ou sans matricule santé) et paiement via une page
  web intégrée.
- Auto-dépistage, conditions d'utilisation, confidentialité, visualisation de PDF, signature électronique.
- **Multilingue** : français, anglais et espagnol (`locale/i18n_*.json`), avec écran de choix de la langue.

## Stack technique

- **Langage** : Dart (SDK `>=2.7.0 <3.0.0`)
- **Framework** : Flutter (Android, iOS)
- **Principales dépendances** : `http`, `shared_preferences`, `flutter_app_lock`, `qr_flutter`,
  `image_picker`, `camera`, `flutter_image_compress`, `flutter_pdfview`, `webview_flutter`,
  `flutter_webview_plugin`, `syncfusion_flutter_signaturepad`, `cached_network_image`, `url_launcher`,
  `permission_handler`, `intl_phone_field`
- **Identifiant Android** : `cm.heatly`

## Structure du projet

```
lib/
├── main.dart        # Point d'entrée : verrouillage, langue, page de démarrage
├── config/          # Configuration (URL de l'API), traductions, singleton, compression d'images
├── pages/           # Écrans (connexion, inscription, accueil, carte digitale, rendez-vous, vaccins…)
├── fragments/       # Sections du tableau de bord (dossier médical, annuaires, assurances, urgence…)
├── forms/           # Formulaires (connexion, inscription, rendez-vous, vaccin, souscription…)
├── models/          # Modèles de données (Compte, Profil, Medecin, Consultation, Vaccin, Abonnement…)
└── lock/            # Écran de verrouillage de l'application
locale/              # Fichiers de traduction (fr, en, es)
img/                 # Images et icônes
android/, ios/       # Projets natifs
```

## Installation et lancement

Prérequis : un SDK Flutter compatible avec Dart 2 (contrainte `>=2.7.0 <3.0.0`), Android Studio / Xcode.
Certaines dépendances étant anciennes, une version de Flutter de la même époque (Flutter 1.x / 2.x)
peut être nécessaire.

```bash
flutter pub get
flutter run                 # lancer sur un appareil ou un émulateur
flutter build apk           # générer l'APK Android
flutter build ios           # générer l'application iOS (sur macOS)
```

## Configuration

Le projet ne contient pas de fichier `.env.example`. Les adresses du serveur back-end (API, images, PDF)
sont définies dans `lib/config/Setting.dart` (`apiracine`, `serveurracine`, `serveurimage`,
`serveurimage1`, `serveurpdf`) et doivent être adaptées à votre environnement avant la compilation.

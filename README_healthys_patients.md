# Healthys Patients

Application mobile Flutter de la famille **Healthys**, destinée aux **patients**. Elle leur permet de
conserver leurs informations de santé en ligne (carte digitale santé, carnet de santé, historique médical,
carnet de vaccination), de trouver des professionnels et structures de santé et de prendre rendez-vous,
en s'appuyant sur l'API back-end Healthys.

Il s'agit de la version la plus récente de l'application patient (voir aussi `healthys_patient` et
`healyhys2`, versions antérieures). Les applications sœurs `helthysfront` (structures et médecins) et
`healthysvendeur` (revendeurs) complètent l'écosystème.

## Fonctionnalités principales

- **Compte** : inscription, connexion, réinitialisation et changement du mot de passe, gestion de
  plusieurs profils rattachés au compte (changement de profil), informations personnelles.
- **Verrouillage de l'application** (`flutter_app_lock`) après une période d'inactivité.
- **Tableau de bord patient** :
  - ma carte digitale santé (avec QR code) et activation de carte ;
  - historique médical, consultations, antécédents médicaux, dossier médical ;
  - paramètres médicaux et mode de vie ;
  - carnet de vaccination (ajout de vaccins) ;
  - informations de première urgence ;
  - mon carnet de santé ;
  - mes rendez-vous (prise, consultation, modification) ;
  - Health News (actualités santé).
- **Espace professionnel** (selon le profil) : carte digitale pro, consultations, dossiers médicaux,
  rendez-vous, identification d'un patient et activation de carte.
- **Annuaires** : personnels de santé (médecins), structures de santé (hôpitaux, centres, pharmacies,
  laboratoires), assurances, courtiers en assurance, banques, partenaires.
- **Abonnements** : souscription à un abonnement (avec ou sans matricule santé) et paiement via une
  page web intégrée.
- Auto-dépistage, conditions d'utilisation et politique de confidentialité.
- Visualisation de PDF, prise de photo / galerie, compression d'images, signature électronique.
- **Multilingue** : français, anglais et espagnol (`locale/i18n_*.json`), avec écran de choix de la langue.

## Stack technique

- **Langage** : Dart (SDK `>=2.18.4 <3.0.0`)
- **Framework** : Flutter (Android, iOS, web, Linux, macOS, Windows)
- **Principales dépendances** : `http`, `shared_preferences`, `flutter_app_lock`, `qr_flutter`,
  `image_picker`, `camera`, `flutter_image_compress`, `flutter_pdfview`, `flutter_webview_plugin`,
  `webview_flutter_plus`, `syncfusion_flutter_signaturepad`, `cached_network_image`, `url_launcher`,
  `permission_handler`, `intl_phone_field`
- **Version de l'application** : 1.0.1+23 — identifiant Android `cm.heatly`

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
android/, ios/, web/, linux/, macos/, windows/   # Projets des différentes plateformes
```

## Installation et lancement

Prérequis : Flutter 3.x avec Dart `>=2.18.4 <3.0.0`, Android Studio / Xcode.

```bash
flutter pub get
flutter run                 # lancer sur un appareil ou un émulateur
flutter build apk           # générer l'APK Android
flutter build ios           # générer l'application iOS (sur macOS)
```

Pour régénérer les icônes de l'application :

```bash
flutter pub run flutter_launcher_icons:main
```

## Configuration

Le projet ne contient pas de fichier `.env.example`. Les adresses du serveur back-end (API, images, PDF)
sont définies dans `lib/config/Setting.dart` (`apiracine`, `serveurracine`, `serveurimage`,
`serveurimage1`, `serveurpdf`) et doivent être adaptées à votre environnement avant la compilation.

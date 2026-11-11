# Health'ys Médecin – Application mobile pour les médecins

Application Flutter (`healthys_medecin`, version `1.0.1+12`) de la plateforme de santé
**Health'ys**, destinée aux médecins. Elle leur permet de consulter le dossier médical d'un
patient (via sa carte digitale santé à QR code), de mener et d'enregistrer des consultations,
de gérer leurs rendez-vous et de suivre la vaccination de leurs patients.

L'application consomme l'API REST de la plateforme Health'ys.

## Fonctionnalités principales

- Choix de la langue (français, anglais, espagnol), écran de démarrage
- Connexion, création de compte, activation, réinitialisation et changement de mot de passe
- Choix du profil, verrouillage de l'application par code (`flutter_app_lock`)
- Carte digitale professionnelle du médecin (QR code)
- Scan du QR code de la carte santé d'un patient pour accéder à son dossier
- Création de patients et de dossiers médicaux
- Consultations : création en plusieurs étapes, historique, détail, évolution, signature
- Antécédents médicaux du patient, groupe sanguin / électrophorèse
- Certificats médicaux et rapports médicaux (affichage PDF)
- Rendez-vous : création, consultation, modification
- Carnet et planning de vaccination, ajout de vaccins
- Documents du patient (upload d'images et de fichiers)
- Demande de paiement (mode et référence de paiement), abonnements et souscriptions
- Actualités (Health News)
- Annuaires des partenaires : hôpitaux, pharmacies, laboratoires, centres d'imagerie,
  assurances, banques, courtiers en assurance
- Paramètres du compte, confidentialité et sécurité, conditions d'utilisation

## Stack technique

- **Flutter** / **Dart** (contrainte SDK `>=2.18.4 <3.0.0`)
- Réseau : `http`, `dio`
- Stockage local : `shared_preferences`
- Internationalisation : fichiers JSON dans `locale/` chargés par `lib/config/all_translations.dart`
- QR code : `qr_flutter` (génération), `mobile_scanner` (lecture)
- Médias et documents : `image_picker`, `multi_image_picker`, `file_picker`,
  `flutter_image_compress`, `flutter_pdfview`, `image_gallery_saver`
- Signature : `flutter_signature_pad`, `syncfusion_flutter_signaturepad`
- Divers : `flutter_app_lock`, `webview_flutter_plus`, `intl_phone_field`,
  `cached_network_image`, `flutter_slidable`, `url_launcher`, `permission_handler`

## Structure du projet

```
lib/
├── config/      # Paramètres (Setting.dart), traductions, singleton, compression d'images
├── fragments/   # Sections de l'application (tableau de bord, consultations, RDV, vaccination, annuaires…)
├── forms/       # Formulaires (connexion, patient, consultation, RDV, certificat, paiement…)
├── lock/        # Écran de verrouillage de l'application
├── models/      # Modèles de données (patient, médecin, consultation, vaccin, abonnement…)
├── pages/       # Écrans de l'application
└── main.dart
img/             # Images et icônes
locale/          # Traductions JSON (fr, en, es)
android/ ios/ web/   # Projets par plateforme
test/            # Tests
```

## Installation et lancement

Prérequis : Flutter SDK compatible avec Dart 2.18 / 2.19 (projet antérieur à Dart 3),
un émulateur ou un appareil connecté.

```bash
flutter pub get          # installer les dépendances
flutter run              # lancer l'application
flutter build apk        # build Android
flutter build ios        # build iOS (sur macOS)
```

Icônes de l'application : `flutter pub run flutter_launcher_icons:main`.

## Configuration

- Aucun fichier `.env.example` n'est fourni.
- Les adresses de l'API et du serveur de fichiers sont définies dans `lib/config/Setting.dart`.

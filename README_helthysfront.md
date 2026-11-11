# Healthys Front (Healthys Hôpital)

Application mobile Flutter de la famille **Healthys**, destinée aux **professionnels et structures de santé**
(personnel administratif, infirmier(e)s, médecins). Elle permet de créer et d'activer des comptes patients,
de gérer les rendez-vous et les consultations, et de consulter les dossiers médicaux des patients, en
s'appuyant sur l'API back-end Healthys.

Elle complète les autres applications de l'écosystème :
- `healthys_patients` / `healthys_patient` : application destinée aux patients ;
- `healthysvendeur` : application des revendeurs (enregistrement de patients et d'entreprises).

## Fonctionnalités principales

- **Connexion** par identifiant (ou numéro d'ordre) et mot de passe, réinitialisation du mot de passe,
  choix d'un profil (« Administration », « Infirmier(e) ») et changement de profil en cours de session.
- **Verrouillage de l'application** (`flutter_app_lock`) après une période d'inactivité en arrière-plan.
- **Deux tableaux de bord selon le rôle de l'utilisateur** :
  - *Espace structure / administration* : créer un compte patient, activer un compte patient,
    nouveau rendez-vous, gestion des rendez-vous, enregistrement d'une entreprise ;
  - *Espace médecin* : nouvelle consultation, gestion des consultations, gestion des rendez-vous,
    dossiers médicaux des patients.
- **Consultations** : création, modification, détail, clôture d'une consultation, rapport médical,
  suivi de l'évolution des paramètres.
- **Rendez-vous** : création, modification, liste et clôture des rendez-vous.
- **États financiers** : affichage de la commission et du solde du compte, demande de paiement
  (avec ou sans signature du patient, via un pad de signature).
- Prise de photo / sélection d'images (caméra, galerie), compression d'images, envoi de fichiers,
  visualisation de PDF, génération de QR codes.
- **Multilingue** : français, anglais et espagnol (`locale/i18n_*.json`), avec écran de choix de la langue.

## Stack technique

- **Langage** : Dart (SDK `>=2.7.0 <3.0.0`)
- **Framework** : Flutter (Android, iOS, web)
- **Principales dépendances** : `http`, `dio`, `shared_preferences`, `flutter_app_lock`, `image_picker`,
  `camera`, `file_picker`, `flutter_image_compress`, `flutter_pdfview`, `qr_flutter`,
  `syncfusion_flutter_signaturepad`, `webview_flutter_plus`, `cached_network_image`, `fluttertoast`,
  `url_launcher`, `permission_handler`, `intl_phone_field`
- **Identifiant Android** : `com.novasos.healthys_hopital`

## Structure du projet

```
lib/
├── main.dart        # Point d'entrée : verrouillage, langue, redirection selon le rôle
├── config/          # Configuration (URL de l'API), traductions, singleton, tailles d'écran
├── page/            # Écrans (connexion, accueils, consultations, rendez-vous, patients, soldes…)
├── form/            # Formulaires (connexion, patient, consultation, rendez-vous, entreprise, paiement…)
├── fragment/        # Fragments d'accueil, rendez-vous, rapport médical
├── model/           # Modèles de données (Compte, Profil, Meeting, Docteur, Consultation…)
└── lock/            # Écran de verrouillage de l'application
locale/              # Fichiers de traduction (fr, en, es)
img/                 # Images et icônes de l'application
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

Pour régénérer les icônes de l'application :

```bash
flutter pub run flutter_launcher_icons:main
```

## Configuration

Le projet ne contient pas de fichier `.env.example`. Les adresses du serveur back-end (API, images, PDF)
sont définies dans `lib/config/Setting.dart` (`apiracine`, `serveurracine`, `serveurimage`,
`serveurimage1`, `serveurpdf`) et doivent être adaptées à votre environnement avant la compilation.

# Healthys 2 (Heatly – démo)

Application mobile Flutter de la famille **Healthys**, version initiale / de démonstration de l'application
Healthys (identifiant Android `cm.heatly_demo`). Elle s'adresse aux **patients** et, selon le profil de
l'utilisateur connecté, aux **professionnels de santé**. Elle permet de conserver ses informations de santé
en ligne (carte digitale, dossier médical, vaccinations), de consulter des annuaires et de gérer des
rendez-vous, en s'appuyant sur l'API back-end Healthys.

Les versions ultérieures de l'application patient se trouvent dans `healthys_patient` puis
`healthys_patients` ; `helthysfront` (structures et médecins) et `healthysvendeur` (revendeurs) complètent
l'écosystème.

## Fonctionnalités principales

- **Compte** : inscription, connexion, réinitialisation et changement du mot de passe.
- **Tableau de bord selon le profil** :
  - *Patient* : ma carte digitale santé, dernière consultation, mode de vie, assurance maladie,
    prise de rendez-vous, annuaire des médecins, carnet de vaccination, carnet de santé ;
  - *Professionnel* : ma carte digitale pro, mes consultations, dossiers médicaux, mes rendez-vous,
    identification d'un patient et activation de carte par **scan de QR code** (`qrscan`).
- **Dossier médical** : consultations, antécédents médicaux, rapport médical.
- **Rendez-vous** : prise, consultation et modification.
- **Annuaires** : médecins, hôpitaux, centres, pharmacies, laboratoires, assurances, courtiers, banques.
- **Abonnements** : souscription (avec ou sans matricule santé).
- Informations de première urgence, actualités santé, conditions d'utilisation.
- **Multilingue** : français, anglais et espagnol (`locale/i18n_*.json`).

## Stack technique

- **Langage** : Dart (SDK `>=2.7.0 <3.0.0`)
- **Framework** : Flutter (Android, iOS)
- **Principales dépendances** : `http`, `shared_preferences`, `qr_flutter`, `qrscan`, `image_picker`,
  `camera`, `webview_flutter`, `flutter_webview_plugin`, `cached_network_image`, `flutter_swiper`,
  `flutter_datetime_picker`, `intl_phone_field`
- **Identifiant Android** : `cm.heatly_demo`

## Structure du projet

```
lib/
├── main.dart        # Point d'entrée de l'application
├── config/          # Configuration (URL de l'API), traductions, tailles d'écran
├── pages/           # Écrans (connexion, inscription, accueil, carte digitale, rendez-vous, vaccins…)
├── fragments/       # Sections du tableau de bord (dossier médical, annuaires, assurances, urgence…)
├── forms/           # Formulaires (connexion, inscription, rendez-vous, vaccin, souscription…)
└── models/          # Modèles de données (Compte, Profil, Medecin, Consultation, Vaccin…)
locale/              # Fichiers de traduction (fr, en, es)
img/                 # Images et icônes
android/, ios/       # Projets natifs
```

Remarque : le dépôt contient aussi des artefacts de compilation (`build/`, `.dart_tool/`, `.packages`,
`.flutter-plugins*`) qui peuvent être régénérés et ne sont pas nécessaires au développement.

## Installation et lancement

Prérequis : un SDK Flutter compatible avec Dart 2 (contrainte `>=2.7.0 <3.0.0`) ; les dépendances étant
anciennes, une version de Flutter de la même époque (Flutter 1.x / 2.x) peut être nécessaire.

```bash
flutter clean               # supprime les artefacts de compilation versionnés
flutter pub get
flutter run                 # lancer sur un appareil ou un émulateur
flutter build apk           # générer l'APK Android
```

## Configuration

Le projet ne contient pas de fichier `.env.example`. Les adresses du serveur back-end sont définies dans
`lib/config/Setting.dart` et doivent être adaptées à votre environnement avant la compilation.

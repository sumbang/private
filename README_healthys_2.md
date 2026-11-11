# Health'ys 2 – Application mobile (patients, médecins, hôpitaux)

Application Flutter de la plateforme de santé **Health'ys** (version `2.0.0+31`, voir `pubspec.yaml`).
Elle permet aux patients de conserver et partager leur carnet de santé numérique, aux médecins
de gérer leurs consultations et le suivi de leurs patients, et au personnel hospitalier
(gestionnaire, infirmier) de gérer l'accueil, les rendez-vous et les consultations d'un établissement.

Il s'agit d'une version antérieure de l'application mobile Health'ys (dépôt `healthys`) :
même architecture, sans les fonctionnalités ajoutées ensuite (contacts d'urgence, service client
par chat, suivi des rendez-vous médecin, localisations ARB…).

L'application consomme l'API REST de la plateforme Health'ys (couche `lib/data/api`).

## Fonctionnalités principales

**Commun**
- Onboarding, choix de la langue (français, anglais, espagnol)
- Inscription, connexion, activation de compte, réinitialisation et changement de mot de passe
- Création d'un profil Patient ou Docteur, choix du profil actif
- Conditions d'utilisation et politique de confidentialité
- Visualisation de documents PDF et d'images
- Notifications push (Firebase Cloud Messaging) et notifications locales
- Vérification des mises à jour de l'application (`upgrader`)

**Patient**
- Carte digitale santé avec QR code
- Profil, antécédents médicaux, documents du dossier médical
- Liste, détail et archives des consultations, évolution d'une consultation
- Prise et liste des rendez-vous (ajout au calendrier du téléphone)
- Annuaires : médecins, hôpitaux, pharmacies, laboratoires ; avis / notation
- Messagerie avec les médecins
- Transfert de profil

**Médecin**
- Tableau de bord, salle d'attente
- Lecture de la carte patient par scan de QR code
- Démarrage et déroulé d'une consultation : antécédents, paramètres médicaux,
  diagnostics, examens, prescriptions de médicaments
- Import de consultations, archives
- Gestion des rendez-vous, messagerie avec les patients
- Mise à jour du profil, carte professionnelle

**Hôpital (gestionnaire / infirmier)**
- Création de patients et gestion de leurs abonnements
- Démarrage et suivi des consultations
- Gestion des rendez-vous de l'établissement

## Stack technique

- **Flutter** / **Dart** (contrainte SDK `>=2.19.6 <3.0.0`)
- Gestion d'état : `hooks_riverpod`, `flutter_hooks` ; injection de dépendances : `get_it`
- Réseau : `dio`
- Firebase : Core, Messaging, Analytics, Crashlytics, Performance
- Stockage local : `shared_preferences`, `sqflite`
- Internationalisation : fichiers JSON dans `locale/` (fr, en, es) chargés par `lib/core/global_translation.dart`
- Divers : `qr_flutter`, `mobile_scanner`, `file_picker`, `image_picker`,
  `flutter_image_compress`, `flutter_cached_pdfview`, `flutter_signature_pad`,
  `add_2_calendar`, `flutter_local_notifications`, `url_launcher`

## Structure du projet

```
lib/
├── core/                 # Paramètres, traductions, navigation, notifications push, utilitaires
├── data/
│   ├── api/              # Client de l'API REST (remote_api.dart)
│   ├── models/request/   # Modèles des requêtes
│   ├── models/response/  # Modèles des réponses
│   └── repository/       # Implémentation des repositories
├── domain/
│   ├── entities/         # Entités métier
│   ├── repository/       # Interfaces des repositories
│   └── usercase/         # Cas d'usage (auth, compte, données)
├── presentation/
│   ├── components/
│   │   ├── view_models/  # View models / états Riverpod
│   │   └── widgets/      # Composants réutilisables
│   └── screens/
│       ├── common/       # Écrans partagés (création de profil, PDF, détails…)
│       └── mobile/       # auth/, patient/, medecin/, hopital/, fragments/
└── main.dart
assets/                   # Polices
img/                      # Images et icônes de l'application
locale/                   # Traductions JSON (fr, en, es)
android/ ios/ web/ linux/ macos/ windows/   # Projets par plateforme
test/                     # Tests
```

## Installation et lancement

Prérequis : Flutter SDK installé (`flutter doctor`), un émulateur ou un appareil connecté.

```bash
flutter pub get          # installer les dépendances
flutter run              # lancer l'application
flutter test             # lancer les tests
```

Génération des builds :

```bash
flutter build apk        # Android
flutter build ios        # iOS (sur macOS)
flutter build web        # Web
```

Icônes de l'application : `dart run flutter_launcher_icons`.

## Configuration

- Aucun fichier `.env.example` n'est fourni.
- L'adresse de base de l'API est définie par la constante `url` de `lib/data/api/remote_api.dart`.
- Les fichiers de configuration natifs Firebase doivent être présents dans `android/` et `ios/`.
- Les autres paramètres de l'application (couleurs, icônes, utilitaires) se trouvent dans `lib/core/setting.dart`.

# E-survey – Application mobile des auditeurs terrain

Application mobile Flutter (Android / iOS) destinée aux **auditeurs terrain** chargés de réaliser des **enquêtes / audits dans des points de vente**. Elle permet de consulter son planning d'audits, de recenser les magasins, de relever les produits présents (prix, quantités, photos) et de transmettre ces données, géolocalisées, à une API REST.

Elle est le client mobile du back-office et de l'API Yii 2 du projet `survey` (ressources `compte`, `stores`, `products`, `surveys`).

## Fonctionnalités principales

- **Connexion / déconnexion** de l'auditeur (jeton conservé localement via `shared_preferences`), rafraîchissement des données.
- Navigation par onglets : **Planning**, **Surveys**, **Stores**, **Products**, **Settings**.
- **Planning** : consultation des audits planifiés pour l'auditeur.
- **Audits (surveys)** : création d'un audit, ouverture, réouverture, clôture et fin d'audit, modification et suppression ; saisie des relevés produits d'un magasin.
- **Magasins** : liste, fiche détaillée et formulaire de recensement d'un nouveau magasin (localisation administrative via listes de référence région / municipalité, caractéristiques du point de vente…).
- **Produits** : liste, recherche, ajout et modification d'articles (catégories, pays, attributs), avec **photos** (appareil photo ou sélection multiple dans la galerie) et envoi au serveur.
- **Géolocalisation** des magasins, audits et relevés produits (`geolocator`).

## Stack technique

- **Flutter** / **Dart** (SDK `>=2.7.0 <3.0.0`, code antérieur au null-safety)
- Principales dépendances : `http`, `shared_preferences`, `geolocator`, `image_picker`, `multi_image_picker`, `cached_network_image`, `flutter_typeahead`, `simple_autocomplete_formfield`, `dropdown_formfield`, `flutter_datetime_picker`, `time_range`, `flutter_slidable`, `flutter_staggered_grid_view`, `webview_flutter`, `fluttertoast`, `font_awesome_flutter`
- Icônes d'application générées avec `flutter_launcher_icons`
- Android : minSdk 16

## Structure du projet

```
lib/
├── main.dart        # Point d'entrée de l'application
├── page/            # Écrans (connexion, accueil, audit, fin/réouverture d'audit, nouveau magasin,
│                    #   détail magasin, nouveau/édition de produit, nouvel audit…)
├── fragment/        # Onglets de l'écran d'accueil (planning, formulaires d'audit, magasins,
│                    #   produits, produits audités, paramètres)
├── form/            # Formulaires (connexion, magasin, produit, audit, fin d'audit, réouverture)
├── model/           # Modèles de données (Store, Product, Audit, AuditProduit, Survey, Domaine,
│                    #   Categorie, Pays, Item)
└── config/          # Configuration (URL de l'API et du serveur d'images dans Setting.dart), tailles d'écran
img/                 # Images de l'application
android/, ios/       # Projets natifs
```

## Installation et lancement

Prérequis : un SDK Flutter compatible avec Dart 2.x (version antérieure à Dart 3) et un émulateur ou un appareil Android / iOS.

```bash
flutter pub get
flutter run
```

Compiler une version de production :

```bash
flutter build apk     # Android
flutter build ios     # iOS
```

## Configuration

Aucun fichier `.env` n'est utilisé. Les URL de l'API REST et du serveur d'images sont définies dans `lib/config/Setting.dart` (`apiracine`, `serveurracine`, `serveurimage`) ; adaptez-les à votre déploiement du back-office `survey` avant de lancer l'application.

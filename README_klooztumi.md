# Klooztumi

Application mobile Flutter (Android / iOS) qui met en relation des **commerces de proximité** et leurs **clients** sous la forme d'un réseau social local. Les commerçants y présentent leur boutique, leurs produits et publient des actualités ; les clients découvrent les commerces autour d'eux, les suivent et interagissent avec leurs publications.

L'application est un client d'une API REST distante (authentification, commerces, produits, publications, utilisateurs).

## Fonctionnalités principales

- **Onboarding** et écran de démarrage (splashscreen).
- **Authentification** : connexion, rafraîchissement de jeton, mot de passe oublié, changement de mot de passe.
- **Deux types de comptes** avec parcours dédiés :
  - **Client** : inscription, fil d'actualité, liste des commerces suivis, contacts, gestion du compte.
  - **Commerçant** : inscription, création d'un commerce (catégories, tags, adresse, coordonnées GPS, horaires d'ouverture, moyens de paiement, site web, bannière et photo de profil), gestion du catalogue de produits (ajout / modification), abonnement / fin d'abonnement.
- **Publications (posts)** avec images : création, détail, commentaires, « j'aime » / « je n'aime plus », partage, suppression.
- **Abonnements** : suivre / ne plus suivre un commerce, favoris, liste des abonnés et des abonnements d'un utilisateur.
- **Recherche** de commerces ou de personnes, avec prise en compte de la position de l'utilisateur.
- **Géolocalisation** (position courante via `geolocator`) et ouverture de l'itinéraire vers un commerce dans l'application de cartographie (`maps_launcher`).
- **QR code** personnel (génération via `qr_flutter`, scan via `qrscan`).
- Sélection / compression d'images, lecture de PDF, choix de fichiers.
- Paramètres : notifications, thème d'affichage, politique de confidentialité, déconnexion.
- Interface en français (fichiers de traduction `locale/i18n_fr.json` et `locale/i18n_en.json`).

## Stack technique

- **Flutter** / **Dart** (SDK `>=2.7.0 <3.0.0`, code antérieur au null-safety)
- Principales dépendances : `http`, `shared_preferences`, `cached_network_image`, `image_picker`, `file_picker`, `flutter_image_compress`, `geolocator`, `flutter_map`, `maps_launcher`, `qr_flutter`, `qrscan`, `flutter_slidable`, `carousel_slider`, `flutter_pdfview`, `permission_handler`, `url_launcher`, `intl_phone_field`, `fluttertoast`, `font_awesome_flutter`
- Icônes d'application générées avec `flutter_launcher_icons`
- Identifiant Android : `com.apps.klooztumi` (minSdk 21)

## Structure du projet

```
lib/
├── main.dart          # Point d'entrée de l'application
├── page/              # Écrans (connexion, inscription client/commerçant, accueil, détail commerce,
│                      #   ajout/édition de produit, publication, recherche, paramètres, abonnement…)
├── fragment/          # Onglets des écrans d'accueil client et commerçant (accueil, boutique,
│                      #   produits, contacts, compte, paramètres)
├── model/             # Modèles de données (User, Seller, Produit, Post, Comment, Tag,
│                      #   CategorieShop, HeureShop, LocationShop, PaymentMethod, Souscription…)
├── block/             # Widgets réutilisables (aperçu de post, image, état vide)
└── config/            # Configuration (URL de l'API dans Setting.dart), widgets communs, utilitaires
assets/                # Polices (Candara, FontAwesome)
img/                   # Images, logos et icônes
locale/                # Fichiers de traduction JSON (fr, en)
android/, ios/, web/   # Projets natifs / plateformes
```

## Installation et lancement

Prérequis : un SDK Flutter compatible avec Dart 2.x (version antérieure à Dart 3) et un émulateur ou un appareil Android / iOS.

```bash
flutter pub get
flutter run
```

Générer les icônes de l'application :

```bash
flutter pub run flutter_launcher_icons:main
```

Compiler une version de production :

```bash
flutter build apk     # Android
flutter build ios     # iOS
```

## Configuration

Aucun fichier `.env` n'est utilisé. L'URL de base de l'API REST est définie dans `lib/config/Setting.dart` (constante `Setting.apiracine`) ; adaptez-la à votre environnement avant de lancer l'application.

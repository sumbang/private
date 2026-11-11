# Health'ys – Back-office et API (Yii 2)

Back-end PHP de la plateforme de santé **Health'ys**. Le projet regroupe :

- un **back-office web** d'administration (patients, médecins, partenaires, abonnements,
  cartes digitales santé, consultations, vaccination…) destiné aux administrateurs de la
  plateforme et aux établissements de santé ;
- une **API REST** (`api/`, module `v1`) consommée par les applications mobiles Health'ys
  (patients et médecins).

Le projet est construit à partir du gabarit *Yii 2 Basic Project Template*.

## Fonctionnalités principales

**Back-office web** (menus selon le rôle de l'utilisateur connecté)
- Tableau de bord
- Administration : patients, médecins, revendeurs, comptes, profils
- Entités : partenaires (hôpitaux, pharmacies, laboratoires…), annuaire, affectation de médecins,
  entreprises, personnel et personnel médical, infirmières
- Abonnements : offres, codes d'abonnement, souscriptions, recharges de compte, paiements,
  ventes et commissions
- Cartes digitales santé : création, affectation, activation, vente, export / impression PDF avec QR code
- Paramètres : spécialités, pays, actualités
- Production : consultations, rendez-vous médicaux, certificats médicaux,
  programmes et planning de vaccination, vaccins enregistrés
- Rapports de consultation en PDF, réinitialisation de mot de passe par e-mail,
  envoi de SMS (Twilio), pages de souscription / paiement en ligne

**API REST v1** (`api/modules/v1/controllers`)
- `compte` : connexion / déconnexion, création et mise à jour de compte, réinitialisation
  du mot de passe, profil, paramètres médicaux et antécédents, informations d'urgence,
  upload de documents, abonnements et souscriptions, recherche de patients et de médecins
- `consultation` : création et mise à jour de consultations, dossier médical, rapports,
  certificats, documents, consultations côté médecin et côté patient
- `vaccin` : vaccins et programmes de vaccination
- `meeting` : rendez-vous (création, confirmation, rejet)
- `partner` : partenaires et entreprises
- `new` : actualités

## Stack technique

- **PHP** (>= 5.6 selon `composer.json` ; image Docker `yiisoftware/yii2-php:7.1-apache`)
- **Yii 2** (`yiisoft/yii2 ~2.0.14`), base de données **MySQL**
- Interface d'administration : `yiister/yii2-gentelella`, `hail812/yii2-adminlte3`,
  `ramosisw/yii2-material-dashboard`, `yiisoft/yii2-bootstrap`, `yiisoft/yii2-jui`
- PDF : `kartik-v/yii2-mpdf` ; QR codes : `chillerlan/php-qrcode`, `2amigos/qrcode-library`
- E-mail : `yiisoft/yii2-swiftmailer` ; SMS : `twilio/sdk`
- Tests : Codeception (`codeception.yml`, `tests/`)

## Structure du projet

```
api/            # Application API REST (config/api.php, modules/v1/controllers)
assets/         # Définition des bundles d'assets
commands/       # Commandes console
config/         # Configuration (web.php, console.php, db.php, params.php, test*.php)
controllers/    # Contrôleurs du back-office web
mail/           # Gabarits d'e-mails
messages/       # Traductions
migrations/     # Migrations de base de données
models/         # Modèles ActiveRecord et modèles de recherche
views/          # Vues du back-office (une par module + layouts/)
web/            # Point d'entrée web (index.php) et ressources publiques
widgets/        # Widgets
uploads/        # Fichiers téléversés
tests/          # Tests Codeception (unit, functional, acceptance)
docker-compose.yml, Vagrantfile   # Environnements de développement
```

## Installation et lancement

Installation des dépendances :

```bash
composer install
```

Base de données : créer une base MySQL, renseigner la connexion dans `config/db.php`, puis :

```bash
php yii migrate
```

Lancement en local :

```bash
php yii serve            # back-office sur http://localhost:8080
```

Avec Docker :

```bash
docker-compose run --rm php composer install
docker-compose up -d     # application accessible sur http://127.0.0.1:8000
```

Tests :

```bash
vendor/bin/codecept run
```

## Configuration

Aucun fichier `.env.example` n'est fourni : la configuration se fait dans les fichiers PHP de `config/` :

- `config/db.php` : connexion à la base de données MySQL
- `config/web.php` : configuration du back-office (clé de validation des cookies, mailer, URL)
- `api/config/api.php` : configuration de l'API (règles REST, mailer)
- `config/params.php` : paramètres applicatifs

Ne versionnez pas de mots de passe ni de clés dans ces fichiers ; utilisez des valeurs propres
à chaque environnement.

# SYGEC

Application web de gestion des congés, absences et permissions du personnel d'une organisation. Elle s'adresse aux services de ressources humaines et aux responsables hiérarchiques : suivi des employés et de l'organigramme, émission et validation des décisions de congé, suivi des périodes de jouissance, des suspensions, arrêts et annulations, et génération des documents associés.

## Fonctionnalités principales

- **Tableau de bord** et menu dynamique selon les habilitations de l'utilisateur
- **Organigramme** : directions, départements, services, emplois
- **Ressources humaines** : fiches employés (catégorie, échelon, contrat, situation matrimoniale…), historique
- **Congés et permissions** :
  - décisions de congé par exercice : création, validation, reprise, annulation, suspension, relance
  - jouissances (périodes de congé effectivement prises), suspensions, arrêts, annulations
  - absences ponctuelles et types d'absence
  - ampliations (destinataires des décisions)
  - génération des décisions et documents au format PDF
- **Import / export** : import de décisions et d'employés depuis des fichiers CSV, export CSV des décisions, absences et employés
- **Documents et téléchargements** : dépôt de pièces jointes et espace de téléchargement
- **Sécurité** : utilisateurs, rôles, habilitations, paramètres ; connexion et réinitialisation du mot de passe par e-mail
- **Commandes console** pour les traitements en masse (imports, génération, envoi de mails, correctifs de données)

## Stack technique

- **PHP** `>= 8.0`
- **Yii 2** (`yiisoft/yii2 ~2.0.14`, gabarit « basic »)
- Interface : Bootstrap (`yii2-bootstrap`), thème AdminLTE (`dmstr/yii2-adminlte-asset`), jQuery UI
- Widgets Kartik : sélecteur de date, plage de dates, Select2, génération PDF (`kartik-v/yii2-mpdf`)
- E-mails : `yii2-swiftmailer`
- Base de données relationnelle (MySQL / MariaDB via PDO) avec migrations Yii
- Tests : Codeception
- Interface en français (`language => 'fr'`)

## Structure du projet

```
assets/        # Bundles d'assets (CSS/JS)
commands/      # Commandes console (imports CSV, exports, génération, mails)
config/        # Configuration web, console et tests (db.php est à créer localement)
controllers/   # Contrôleurs web (employés, décisions de congé, jouissances, absences, rôles…)
mail/          # Gabarits d'e-mails
migrations/    # Migrations de base de données
models/        # Modèles ActiveRecord et modèles de recherche (*Search)
views/         # Vues par contrôleur et layouts AdminLTE
web/           # Point d'entrée (index.php) et ressources publiques
widgets/       # Widgets personnalisés
tests/         # Tests Codeception
old/           # Anciens scripts de mise à jour de la base
```

## Installation et lancement

Prérequis : PHP 8, Composer, un serveur de base de données MySQL/MariaDB.

```bash
composer install
```

Créez le fichier `config/db.php` (non versionné), par exemple :

```php
<?php
return [
    'class' => 'yii\db\Connection',
    'dsn' => 'mysql:host=localhost;dbname=<nom_de_la_base>',
    'username' => '<utilisateur>',
    'password' => '<mot_de_passe>',
    'charset' => 'utf8',
];
```

Le dépôt ne contient que quelques migrations incrémentales (directions, services, mises à jour des décisions) : le schéma de base doit être importé au préalable. Appliquez ensuite les migrations puis lancez le serveur de développement :

```bash
php yii migrate
php yii serve      # http://localhost:8080
```

Avec Docker (image `yiisoftware/yii2-php`) :

```bash
docker-compose run --rm php composer install
docker-compose up -d   # http://127.0.0.1:8000
```

Exemples de commandes console :

```bash
php yii employe/import <fichier.csv>
php yii decisionconges/import <fichier.csv> <exercice>
php yii export-data/decision
php yii export-data/absence
```

Tests :

```bash
vendor/bin/codecept run
```

## Configuration

Il n'y a pas de fichier `.env.example`. La configuration se trouve dans `config/` :

- `config/db.php` : connexion à la base de données (à créer, ignoré par Git)
- `config/web.php` / `config/console.php` : composants de l'application (clé de validation des cookies, mailer SMTP…) — renseignez vos propres valeurs
- `config/params.php` : paramètres applicatifs

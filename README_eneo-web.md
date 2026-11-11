# eneo-web : back-office et API des relevés de compteurs terrain

Application web de gestion des **rapports de terrain** produits par les agents chargés du relevé et du contrôle des compteurs électriques. Elle sert à la fois :

- de **back-office** (interface d'administration) pour les superviseurs : consultation des rapports, gestion des utilisateurs, des régions, des agences et des fichiers de données ;
- de **webservice** pour l'application mobile des agents : authentification, téléchargement des données de référence, envoi des rapports de terrain.

Public visé : équipes de supervision (back-office) et agents de terrain (via l'application mobile qui consomme le webservice).

## Fonctionnalités principales

### Back-office (Yii 2)
- Connexion / déconnexion des utilisateurs, avec menus différents selon le rôle.
- **Tableau de bord** avec graphiques « Performances de terrain » et « Volumes d'anomalies ».
- **Rapports de terrain** : liste, recherche, consultation, modification, suppression et **export CSV** (anomalie, usage, index lu, ampérage, latitude/longitude, localisation, compteur, observations, matricule, date d'opération, distance de saisie).
- **Utilisateurs** : gestion des agents (matricule, nom, téléphone, région, agence, statut, rôle).
- **Régions** et **Agences** : gestion des référentiels géographiques.
- **Mise à jour des données** : téléversement de fichiers de données par région (stockés dans `web/uploads/`), mis à disposition de l'application mobile.

### Webservice (`webservice/controller.php`)
Point d'entrée unique piloté par le paramètre `action` :
- `login` : authentification d'un agent par matricule ;
- `getdata` : récupération du fichier de données de la région de l'agent ;
- `setdata` : enregistrement d'un rapport de terrain (anomalie, usage, index, ampérage, position GPS, compteur, observations), avec vérification de la correspondance compteur / contrat ;
- `amperage`, `anomalies` : listes de référence ;
- `compteur`, `grand`, `recherche`, `recherche1` : recherche de compteurs, de contrats, de grands comptes et de points de livraison ;
- `token` : gestion du jeton de l'appareil.

### Scripts d'import
- `compteur_to_contrat.php` : import d'un fichier CSV de correspondance compteurs / contrats.
- `grand_compte.php` : import d'un fichier CSV du référentiel des grands comptes.

## Stack technique

- **PHP** >= 5.4 (image Docker fournie : PHP 7.1 + Apache)
- **Yii 2** (~2.0.14), gabarit « basic »
- **yii2-bootstrap**, **AdminLTE** (`dmstr/yii2-adminlte-asset`), **yii2-swiftmailer**
- Widgets **Kartik** (date-range, datepicker)
- Base de données **MySQL**
- Tests : **Codeception**

## Structure du projet

```
assets/        Bundles d'assets Yii
commands/      Commandes console
config/        Configuration (web, console, base de données, paramètres)
controllers/   Contrôleurs : Site, Rapport, Users, Region, Agence, Datas
models/        Modèles ActiveRecord et modèles de recherche
views/         Vues (layouts AdminLTE, CRUD par entité, tableau de bord)
webservice/    API PHP consommée par l'application mobile
web/           Racine web publique (index.php, uploads)
widgets/       Widgets réutilisables
tests/         Tests Codeception
*.php (racine) Scripts d'import CSV
```

## Installation et lancement

1. Installer les dépendances :
   ```bash
   composer install
   ```
2. Configurer la connexion à la base MySQL dans `config/db.php` (et dans `webservice/` pour l'API).
3. Lancer l'application :
   - avec le serveur intégré :
     ```bash
     php yii serve
     ```
   - ou avec Docker :
     ```bash
     docker-compose up -d
     ```
     L'application est alors accessible sur `http://localhost:8000`.
4. Lancer les tests :
   ```bash
   vendor/bin/codecept run
   ```

## Configuration

Le projet ne fournit pas de fichier `.env.example`. La configuration se fait dans les fichiers du dossier `config/` (`db.php`, `params.php`, `web.php`) et dans les fichiers de connexion du dossier `webservice/`. Ne versionnez pas de mots de passe dans ces fichiers.

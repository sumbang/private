# Survey – Back-office et API d'audits de points de vente

Application web PHP (Yii 2) de gestion d'**enquêtes / audits terrain dans des points de vente** (magasins, boutiques, débits de boissons…). Elle se compose :

- d'un **back-office d'administration** (thème AdminLTE) permettant de gérer les périodes, plannings, auditeurs, comptes, magasins, produits et audits ;
- d'une **API REST** (`api/`, module `v1`) consommée par l'application mobile des auditeurs, qui y envoie les données collectées sur le terrain (recensement des magasins, relevés de produits, photos, géolocalisation).

Public visé : les équipes de supervision qui planifient et suivent les campagnes d'audit, et les auditeurs terrain via l'application mobile.

## Fonctionnalités principales

### Back-office (`controllers/`, `views/`)

- **Tableau de bord** et authentification des administrateurs.
- **Gestion des périodes** d'audit (libellé, date de début et de fin).
- **Gestion des plannings** : affectation de magasins à un auditeur sur une période, date et heure de début, suivi des dates/heures réelles de démarrage et de fin, commentaires ; **import de plannings** depuis un fichier.
- **Gestion des auditeurs** (nom, téléphone, e-mail, code) et des **comptes** utilisateurs (rôles, photo).
- **Gestion des magasins** : fiche détaillée (région, municipalité, ville, zone, urbanité, coordonnées GPS, enseigne, type de magasin, surface, adresse, type d'activité, nombre de caisses, système de réfrigération, consommation sur place, catégories de produits, propriétaire / chaîne, contacts, horaires, local de stockage, type de service, système de facturation…), historique des passages par planning, **export CSV**.
- **Gestion des produits / articles** (catégorie, code, marque, origine, variante, saveur, conditionnement, poids, fabricant, pays, caractère alcoolisé, photo…).
- **Gestion des audits** (« Survey management ») : statut, type, dates et heures, auditeur, magasin, planning ; **export CSV**.

### API REST (`api/modules/v1/controllers/`)

- `compte` : connexion / déconnexion, rafraîchissement, listes de référence (domaines, éléments parents, municipalités).
- `store` : liste et création des magasins, envoi de photos (`uploader`, `uploaders`).
- `product` : liste des produits, catégories, pays, filtres par marque / catégorie, envoi de photos.
- `survey` : plannings de l'auditeur (`plan`), création d'un audit, saisie des relevés produits (quantité, prix, photo, position GPS), ouverture / réouverture / clôture / fin d'audit, modification et suppression.

### Scripts d'import (`import/`)

Scripts PHP autonomes pour importer en base des produits et des magasins depuis des fichiers CSV.

## Stack technique

- **PHP** ≥ 5.6 (image Docker fournie : PHP 7.1 + Apache)
- **Yii 2** (`yiisoft/yii2` ~2.0.14), gabarit « basic » avec un point d'entrée API séparé
- `yiisoft/yii2-bootstrap`, `yiisoft/yii2-swiftmailer`
- `dmstr/yii2-adminlte-asset` (thème AdminLTE du back-office)
- Widgets Kartik : `yii2-widget-datepicker`, `yii2-widget-select2`, `yii2-widget-timepicker`
- Base de données MySQL (via la configuration Yii)
- Tests : Codeception

## Structure du projet

```
api/                 # Application API REST (point d'entrée api/index.php, config/api.php, modules/v1)
assets/              # Bundles d'assets Yii
commands/            # Commandes console
config/              # Configuration (web.php, console.php, db.php, params.php, test*.php)
controllers/         # Contrôleurs du back-office
import/              # Scripts d'import CSV (produits, magasins)
mail/                # Gabarits d'e-mails
models/              # Modèles ActiveRecord et modèles de recherche (Store, Product, Planning,
                     #   Periode, Auditeur, Compte, AuditStore, AuditProduit, Domaine…)
runtime/             # Fichiers générés à l'exécution
tests/               # Tests Codeception
uploads/             # Fichiers et photos envoyés
views/               # Vues du back-office (layouts AdminLTE inclus)
web/                 # Point d'entrée web (index.php) et ressources publiques
docker-compose.yml   # Environnement Docker PHP/Apache
Vagrantfile, vagrant/ # Environnement Vagrant
```

## Installation et lancement

1. Installer les dépendances :

   ```bash
   composer install
   ```

2. Créer une base de données MySQL et renseigner la connexion dans `config/db.php` (et `config/test_db.php` pour les tests).

3. Lancer l'application :

   - avec le serveur intégré de Yii :

     ```bash
     php yii serve --docroot=web          # back-office
     php -S localhost:8081 -t api         # API REST
     ```

   - ou avec Docker :

     ```bash
     docker-compose run --rm php composer install
     docker-compose up -d
     ```

     Le conteneur expose l'application sur le port `8000` (`http://127.0.0.1:8000`).

4. Lancer les tests :

   ```bash
   vendor/bin/codecept run
   ```

## Configuration

Aucun fichier `.env.example` n'est fourni. La configuration se fait dans les fichiers PHP de `config/` (connexion à la base dans `db.php`, paramètres applicatifs dans `params.php`) et `api/config/api.php` pour l'API. Les scripts de `import/` contiennent leurs propres paramètres de connexion à adapter avant usage. Pensez à désactiver `YII_DEBUG` / `YII_ENV=dev` dans `web/index.php` et `api/index.php` en production.

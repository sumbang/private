# I-GEAR : gestion d'un garage automobile et d'un parc de véhicules

I-GEAR est une application web de gestion pour une activité automobile : atelier de réparation, vente de véhicules neufs et d'occasion, location de véhicules et gestion des pièces détachées. Elle couvre le cycle commercial (devis, factures, avoirs), les achats auprès des fournisseurs, les stocks, la caisse, le personnel et l'agenda.

Public visé : le personnel administratif, commercial et technique d'un garage ou d'une concession automobile.

## Fonctionnalités principales

- **Authentification et droits** : connexion, déconnexion, réinitialisation du mot de passe, profil utilisateur. Les rôles et les habilitations définissent les menus accessibles à chaque utilisateur.
- **Tableau de bord** et pages de **statistiques**. Des écrans de synthèse existent par domaine : caisse, clients, devis, factures, fournisseurs, location, stock et véhicules.
- **Véhicules** : fiches véhicules neufs ou d'occasion (prix d'achat et de vente, TVA, TVA sur marge, marge, remise), immatriculations, marques, genres, carrosseries, couleurs et énergies.
- **Atelier** : main-d'œuvre (tarifs, TVA, comptes comptables), forfaits et traitements (interventions rattachées à une immatriculation).
- **Articles et stocks** : articles, familles d'articles, ingrédients et gestion du stock.
- **Achats** : fournisseurs et commandes (validation, rejet, quantités commandées et livrées, règlement).
- **Ventes** : clients, devis (validation, clôture, transformation en facture), factures clients et fournisseurs (statut, clôture, génération **PDF**) et avoirs.
- **Location** : contrats de location (dates, kilométrage et carburant au départ et au retour, assurance, prix HT et TVA) et assurances.
- **Caisse** : mouvements de caisse (entrées et sorties).
- **Immobilisations** : suivi des immobilisations.
- **Personnel et agenda** : fiches du personnel (fonction, matricule, ancienneté), comptes utilisateurs, agenda des tâches affectées, zones.
- **Configuration** et paramètres de l'application.

## Stack technique

- **PHP** >= 5.4. Une image Docker PHP + Apache est fournie par le gabarit Yii.
- **Yii 2** (~2.0.14), gabarit « basic ».
- **AdminLTE** (`dmstr/yii2-adminlte-asset`) et **yii2-bootstrap**.
- Widgets **Kartik** : select2 et **mPDF** (génération des documents PDF).
- **yii2-swiftmailer** pour l'envoi d'e-mails.
- Base de données relationnelle (MySQL) via ActiveRecord.
- Tests : **Codeception**.

## Structure du projet

```
assets/        Bundles d'assets
commands/      Commandes console
config/        Configuration (web, console, base de données, paramètres)
controllers/   Contrôleurs (un par domaine : Vehicule, Devis, Facture, Location, Stock…)
models/        Modèles ActiveRecord, modèles de recherche et formulaires
views/         Vues (layouts AdminLTE, écrans CRUD par entité)
web/           Racine web publique
widgets/       Widgets réutilisables
tests/         Tests Codeception
```

## Installation et lancement

1. Installer les dépendances :
   ```bash
   composer install
   ```
2. Configurer la base de données dans `config/db.php`.
3. Lancer l'application :
   - avec le serveur intégré de Yii :
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

Le dépôt ne contient pas de fichier `.env.example`. La configuration se fait dans les fichiers du dossier `config/` (`db.php`, `params.php`, `web.php`). Ne versionnez pas de mots de passe dans ces fichiers.

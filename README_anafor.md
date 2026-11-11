# ANAFOR : gestion de la production, des stocks, des ventes et du recouvrement

Application web de gestion commerciale et de production développée pour l'ANAFOR. Elle permet de suivre la production des sites (quantités en m³), les stocks et leurs mouvements, la cotation et la facturation des clients, puis le recouvrement des paiements. Le projet fournit aussi une **API REST (v1)** qui expose les mêmes données à des clients externes, par exemple une application mobile.

Public visé : les équipes internes (opérateurs, valideurs, responsables commerciaux et de production) qui travaillent par site, base et antenne.

## Fonctionnalités principales

- **Authentification** : connexion, déconnexion, réinitialisation du mot de passe par e-mail.
- **Interface bilingue** français / anglais, avec un sélecteur de langue.
- **Menus dynamiques selon les habilitations** : les entrées de menu viennent de la base et dépendent du rôle de l'utilisateur. Elles sont regroupées en « Traitements de base », « Opérations de production », « Opérations de ventes » et « Opérations de reporting ».
- **Référentiels** : régions, villes, sites, unités, familles (catégories) de produits, produits, prix, exercices, rôles, utilisateurs, paramètres, taxes, natures de clients et modes de paiement.
- **Production** : inventaires de production par exercice, site et produit (stock initial, objectif en m³), avec validation et annulation.
- **Stocks et mouvements** : entrées et sorties de stock avec validation et annulation, bilans, état des stocks et export.
- **Ventes et facturation** : cotation, validation ou rejet d'une demande, génération de la facture en **PDF**, bilans, état et export.
- **Recouvrement** : enregistrement des paiements, validation, annulation et calcul du reste à payer.
- **Tableau de bord** avec des indicateurs : factures en attente et alertes.
- **API REST v1** (`api/`) : ressources `user` (login, resetpwd), `client`, `facture` (valider, rejeter, buildfacture, search), `cotation`, `commentaire`, `document`, `validation`, `stock`, `produit`, `famille`, `unite`, `taxe`, `ville`, `sites`, `roles`, `exercice`, `recouvrement`, `paiementmode`, `natureclient` et `setting`.

## Stack technique

- **PHP** >= 5.4. Une image Docker PHP 7.1 + Apache est fournie par le gabarit Yii.
- **Yii 2** (~2.0.14), gabarit « basic », plus une application API séparée qui utilise `yii\rest`.
- **AdminLTE** (`dmstr/yii2-adminlte-asset`), **yii2-bootstrap** et **yii2-jui**.
- Widgets **Kartik** : alert, select2, datepicker, date-range et **mPDF** (génération PDF).
- E-mails : **yii2-swiftmailer** et **SendGrid** (`wadeshuler/yii2-sendgrid`).
- Base de données relationnelle (MySQL) via ActiveRecord.
- Tests : **Codeception**.

## Structure du projet

```
api/           API REST (point d'entrée api/index.php, config, module v1 et ses contrôleurs)
assets/        Bundles d'assets
commands/      Commandes console
config/        Configuration de l'application web et console
controllers/   Contrôleurs du back-office (Facture, Stocks, Production, Recouvrement, référentiels…)
messages/      Traductions (fr-FR, en-US)
models/        Modèles ActiveRecord, modèles de recherche et formulaires
views/         Vues (layouts AdminLTE, écrans par entité)
web/           Racine web publique
widgets/       Widgets réutilisables
tests/         Tests Codeception
```

## Installation et lancement

1. Installer les dépendances :
   ```bash
   composer install
   ```
2. Configurer la base de données dans `config/db.php` et les paramètres de l'API dans `api/config/api.php`.
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
4. Pour l'API, faire pointer un hôte virtuel vers le dossier `api/` (point d'entrée `api/index.php`).
5. Lancer les tests :
   ```bash
   vendor/bin/codecept run
   ```

## Configuration

Le dépôt ne contient pas de fichier `.env.example`. La configuration se fait dans les fichiers PHP du dossier `config/` (`db.php`, `params.php`, `web.php`) et dans `api/config/api.php`. Le service d'envoi d'e-mails y est aussi configuré. Ne versionnez pas de mots de passe ni de clés d'API dans ces fichiers.

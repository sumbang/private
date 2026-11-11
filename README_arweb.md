# arweb : back-office et API de suivi des maintenances d'équipements

arweb est une application web de **suivi des opérations de maintenance** sur les équipements installés chez des clients. L'équipement est organisé par client, puis par adresse, par pièce et par élément. L'application comprend :

- un **back-office** pour les administrateurs : gestion des techniciens, des maintenances, des listes de contrôle, des utilisateurs, et consultation de la piste d'audit ;
- une **API REST (v1)** pour l'application mobile des techniciens : connexion par code, consultation des équipements, enregistrement des interventions avec photos avant et après, et statistiques.

Public visé : les gestionnaires et administrateurs (back-office) et les techniciens de maintenance (via l'API).

## Fonctionnalités principales

### Back-office
- Connexion, déconnexion et profil de l'utilisateur.
- **Accueil** : tableau de bord avec graphiques (Chart.js, Google Charts).
- **Techniciens** : gestion des techniciens rattachés à une adresse client, avec un code secret et un statut de technicien principal.
- **Maintenances** : liste, consultation, modification, suppression et **export** des maintenances réalisées. Chaque maintenance enregistre l'élément, le technicien, la date, la nature, le type, un commentaire et les images avant et après.
- **Liste de maintenance** : référentiel des opérations de maintenance par catégorie et par nature.
- **Piste d'audit** : journal des actions des techniciens (date, action, message, appareil).
- **Utilisateurs** : gestion des comptes du back-office.

### API REST v1 (`modules/api/v1`)
- Authentification : `login`, `logout`, `secret`, `resetpwd`, `newpass`.
- Données : `pieces`, `equipement`, `listes`, `list-maintenance`, `techniciens`.
- Gestion des techniciens : `new-tech`, `remove-tech`.
- Interventions : `new-maintenance`, envoi de photos (`uploader`, `uploader1`).
- Statistiques : `stats`.

### Modèle de données
Les principales entités sont : clients, adresses, pièces, éléments (équipements avec photo et caractéristiques), catégories (avec la durée de maintenance), groupes, rôles, techniciens, maintenances, listes de maintenance et piste d'audit.

## Stack technique

- **PHP** >= 5.6
- **Yii 2** (~2.0.14), gabarit « basic », avec un module API REST (`yii\rest`)
- **Bootstrap 4** (`yii2-bootstrap4`) et **AdminLTE** (`dmstr/yii2-adminlte-asset`, `yidas/yii2-adminlte`)
- Graphiques : **Chart.js** (`practically/yii2-chartjs`) et **Google Charts** (`scotthuangzl/yii2-google-chart`)
- **yii2-swiftmailer** pour l'envoi d'e-mails
- Base de données relationnelle (MySQL) via ActiveRecord
- Tests : **Codeception** 4

## Structure du projet

```
assets/            Bundles d'assets
commands/          Commandes console
config/            Configuration (web, console, base de données, paramètres)
controllers/       Contrôleurs du back-office (Site, Technicien, Maintenance, Maintenancelist, Piste, Compte)
models/            Modèles ActiveRecord et modèles de recherche
modules/api/v1/    Module API REST consommé par l'application mobile
views/             Vues (layouts AdminLTE, écrans par entité)
web/               Racine web publique
widgets/           Widgets réutilisables
tests/             Tests Codeception
```

## Installation et lancement

1. Installer les dépendances :
   ```bash
   composer install
   ```
2. Configurer la base de données dans `config/db.php` et le service d'envoi d'e-mails dans `config/web.php`.
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
4. L'API est exposée par le module `apiV1` grâce aux règles REST définies dans `config/web.php` (URL « propres » activées).
5. Lancer les tests :
   ```bash
   vendor/bin/codecept run
   ```

## Configuration

Le dépôt ne contient pas de fichier `.env.example`. La configuration se fait dans les fichiers du dossier `config/` (`db.php`, `params.php`, `web.php`). Ne versionnez pas de mots de passe ni d'identifiants SMTP dans ces fichiers. Gardez-les plutôt dans un fichier local non suivi par Git ou dans des variables d'environnement.

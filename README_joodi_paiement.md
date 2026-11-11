# Joodi Paiement

Passerelle de paiement web de **Joodi**, la plateforme de podcasts africains. Ce petit site PHP
affiche les pages de paiement auxquelles les applications Joodi redirigent l'utilisateur pour
régler un achat (abonnement, contenu…), traite le paiement auprès du prestataire choisi puis
notifie l'API Joodi du résultat de la transaction.

Public visé : les utilisateurs finaux des applications Joodi (paiement) et l'équipe technique
Joodi (déploiement et maintenance de la passerelle).

## Fonctionnalités principales

- **Paiement par carte bancaire via Stripe** (`stripe.php` → `stripe-processing.php` →
  `stripe-process.php`) : formulaire de carte, création/confirmation du paiement, puis
  envoi du statut final à l'API Joodi.
- **Paiement Mobile Money** (`momo.php` → `momo-processing.php`) : le prestataire est choisi
  selon le pays de l'utilisateur :
  - My-CoolPay pour le Cameroun (XAF) ;
  - Monetbil pour le Bénin, le Congo, la RD Congo et le Sénégal ;
  - MyPVit pour le pays associé au code `AM`.
  Seuls les paiements en FCFA sont acceptés sur ce parcours.
- **Retour PayPal** (`succes-paypal.php`) : capture de la commande PayPal après validation par
  l'acheteur et mise à jour du paiement côté Joodi.
- **Confirmation Mobile Money** (`success_momo.php`) : vérification de la transaction auprès du
  prestataire (Lygos) avant confirmation.
- Pages de résultat : `success.php`, `error.php`, `error_momo.php`, `process.php`
  (paiement en cours) et `index.php` (adresse invalide).
- Choix de l'environnement cible par le paramètre `serveur` (`DEMO` ou production) pour
  interroger l'API Joodi correspondante.
- Client HTTP utilitaire (`HttpClient.php`) : requêtes GET/POST, authentification Basic ou
  Bearer, génération d'UUID v4.

## Stack technique

- PHP 8.1 (image Docker `php:8.1-cli`, serveur PHP intégré)
- Composer 2 ; dépendance `stripe/stripe-php` ^15.10
- Front : Bootstrap, jQuery, Font Awesome, Ionicons (fichiers statiques locaux)
- Tests : PHPUnit ^9
- CI/CD : GitHub Actions (tests, analyse, construction d'une archive et d'une image Docker,
  publication sur un dépôt d'artefacts, promotion snapshot → release)

## Structure du projet

```
.
├── index.php, stripe*.php, momo*.php, succes*.php, error*.php, process.php   # pages et traitements
├── HttpClient.php        # client HTTP (namespace App\Utils)
├── Footer.php            # pied de page commun
├── css/, js/, img/       # ressources statiques
├── tests/                # tests PHPUnit des scripts de traitement
├── phpunit.xml           # configuration PHPUnit
├── Dockerfile            # image d'exécution
└── .github/workflows/    # pipelines CI/CD (snapshot et release)
```

## Installation et lancement

Prérequis : PHP 8.1+ avec l'extension cURL, Composer.

```bash
composer install
php -S 0.0.0.0:8000 -t .
```

Le site est alors accessible sur `http://localhost:8000`. Les pages de paiement attendent des
paramètres dans l'URL (par ex. `stripe.php?item=<id_paiement>&serveur=DEMO`).

Avec Docker :

```bash
docker build -t joodi-payment .
docker run -p 8000:8000 --env-file .env joodi-payment
```

Tests :

```bash
vendor/bin/phpunit
```

## Configuration

Les identifiants des prestataires sont lus dans des variables d'environnement (aucun fichier
`.env.example` n'est fourni) :

| Variable | Usage |
| --- | --- |
| `STRIPE_SK_PROD` | Clé secrète Stripe |
| `PAYPAL_USERNAME` / `PAYPAL_PASSWORD` | Identifiants client de l'API PayPal |
| `MONETBILL_SERVICE` | Identifiant de service Monetbil |
| `MYPVIT_MARCHAND_CODE` / `MYPVIT_TOKEN` | Code marchand et jeton MyPVit |
| `LYGOS_API_KEY` | Clé d'API Lygos |

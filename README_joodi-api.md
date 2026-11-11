# Joodi API

API REST de la plateforme **Joodi** (podcasts africains). Elle sert les applications mobiles
Joodi (catalogue audio, lecture, comptes, abonnements, achats) et la passerelle de paiement
web, qui l'interroge pour récupérer et clôturer les paiements.

Public visé : les développeurs des applications clientes Joodi et l'équipe d'exploitation.

## Fonctionnalités principales

API **v1** (`/api/v1/...`, consommée par les applications) :

- **Authentification** (`/auth`) : inscription, connexion (jeton JWT), réinitialisation et
  changement de mot de passe, référentiels publics (langues, pays, domaines).
- **Compte** (`/account`) : activation par code, vérification de jeton, profil, mise à jour,
  suppression du compte, déconnexion, notifications.
- **Audio** (`/audio`) : contenus de l'accueil par rubrique, slider, catégories, recherche,
  détail d'un podcast et de ses épisodes, enregistrement des lectures et téléchargements,
  notation et « j'aime ».
- **Paiement** (`/paiement`) : formules d'abonnement, initialisation d'un abonnement ou d'une
  commande, validation de codes promo, historique des achats, abonnements et paiements.

API **v0** (`/api/v0/...`) :

- **Audio** : tendances, boutique, détail d'un contenu, lectures.
- **Payment** : consultation d'un paiement, mise à jour de sa référence et clôture
  (utilisé par la passerelle `joodi_paiement`).

Les routes non publiques sont protégées par JWT (Spring Security).

## Stack technique

- Java 17, Spring Boot 3.5 (Web/Data REST, Security)
- Bibliothèque partagée `africa.joodi:joodi-common` 2.0.1 (entités JPA, sécurité, S3, e-mail)
- PostgreSQL (via joodi-common), OkHttp, org.json, Lombok
- Tests : Spring Boot Test / JUnit 5 ; couverture JaCoCo
- Build : Maven (wrapper fourni) ; image Docker basée sur OpenJDK 17
- CI/CD : GitHub Actions (build, tests, image Docker, publication sur un dépôt d'artefacts)

## Structure du projet

```
src/main/java/africa/joodi/api/
├── JoodiApiApplication.java
├── config/            # sécurité, jeton d'accès PayPal
├── v0/controllers/    # API v0 (audio, paiement pour la passerelle)
└── v1/controllers/    # API v1 (auth, compte, audio, paiement)
    v1/models/         # requêtes et réponses JSON
src/main/resources/
├── application.properties          # configuration principale
└── application-common.properties   # propriétés transmises à joodi-common
src/test/                           # tests des contrôleurs
Dockerfile, settings.xml            # image d'exécution, configuration Maven du dépôt d'artefacts
```

## Installation et lancement

Prérequis : JDK 17, accès à la dépendance `joodi-common` (installée localement via
`./mvnw install` dans le dépôt `joodi-common`, ou depuis le dépôt d'artefacts), une base
PostgreSQL.

```bash
./mvnw clean package
java -jar target/joodi-api-*.jar
```

ou en développement :

```bash
./mvnw spring-boot:run
```

Le service écoute sur le port `8080`.

Avec Docker (après avoir copié le JAR sous le nom `joodi-api.jar` à la racine) :

```bash
docker build -t joodi-api .
docker run -p 8080:8080 --env-file .env joodi-api
```

## Configuration

Aucun fichier `.env.example` n'est fourni. Les variables d'environnement lues sont :

- Base de données : `DATABASE_URL`, `DATABASE_USER`, `DATABASE_PASSWORD`, `DATABASE_SCHEMA`
- SMTP : `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASSWORD`
- Sécurité : `JWT_SECRET`
- AWS : `S3_BUCKET`, `S3_REGION`, `S3_ACCESS_KEY`, `S3_SECRET_KEY`,
  `CLOUDFRONT_DISTRIBUTION_ID`, `CLOUDFRONT_KEY_PAIR`, `CLOUDFRONT_PRIVATE_KEY`
- Paiement : `PAYPAL_TOKEN`, `LYGOS_TOKEN`
- Divers : `APPS_URL`, `BILL_URL`, `SERVEUR` (environnement cible)

# Joodi Backend

Console d'administration web de la plateforme **Joodi** (podcasts africains). Elle permet à
l'équipe Joodi de gérer le catalogue (podcasts, épisodes, catégories, publications), les
utilisateurs, les offres commerciales et de suivre l'activité de la plateforme.

Public visé : administrateurs et opérateurs de Joodi.

## Fonctionnalités principales

- **Authentification** par formulaire (Spring Security), avec rôles ; la gestion des comptes
  et l'envoi de notifications sont réservés au rôle `Administrateur`. Réinitialisation de mot
  de passe par e-mail.
- **Catalogue** : podcasts et leurs épisodes (envoi des fichiers audio et images vers
  Amazon S3), catégories, langues, publications par plateforme et zone,
  bannières du slider.
- **Utilisateurs** : comptes (création, modification, profil, import et export), personnes.
- **Offres et ventes** : formules d'abonnement, codes promo, souscriptions, achats, paiements
  (listes paginées et recherche multicritère).
- **Notifications** : envoi de notifications aux utilisateurs des applications.
- **Tableau de bord et statistiques** (`exchange`) : utilisateurs, revenus, codes, souscriptions,
  achats, paiements, écoutes et téléchargements.
- **Exports et imports** (`export`) : extraction des mêmes données au format Excel/CSV ;
  import en masse d'utilisateurs, de souscriptions et d'achats depuis un fichier Excel.

## Stack technique

- Java 17, Spring Boot 3.5 (Web, Data JPA, Data REST, Security, Mail)
- Thymeleaf (layout dialect, extras Spring Security) ; thème AdminLTE, Bootstrap, jQuery
- Bibliothèque partagée `africa.joodi:joodi-common` 2.0.1 (entités, dépôts, S3, e-mail)
- PostgreSQL (via joodi-common)
- Apache POI et Commons CSV (exports), Lombok
- Tests : Spring Boot Test / JUnit 5 ; couverture JaCoCo
- Build : Maven (wrapper fourni) ; image Docker basée sur OpenJDK 17
- CI/CD : GitHub Actions (build, tests, image Docker, publication sur un dépôt d'artefacts)

## Structure du projet

```
src/main/java/africa/joodi/backend/
├── JoodiBackendApplication.java
├── config/        # configuration de sécurité
├── controllers/   # contrôleurs MVC (podcast, compte, abonnement, export, exchange…)
├── services/      # logique métier et helper Excel
└── search/        # critères de recherche des listes
src/main/resources/
├── templates/     # vues Thymeleaf par module (podcast/, compte/, dashboard/…)
├── static/        # ressources AdminLTE, Bootstrap, plugins JS
├── application.properties
└── application-common.properties
src/test/          # tests unitaires
```

## Installation et lancement

Prérequis : JDK 17, accès à la dépendance `joodi-common` (installée localement via
`./mvnw install` dans le dépôt `joodi-common`, ou depuis le dépôt d'artefacts), une base
PostgreSQL.

```bash
./mvnw clean package
java -jar target/joodi-backend-*.jar
```

ou en développement :

```bash
./mvnw spring-boot:run
```

L'interface est servie sur le port `8080` (page de connexion : `/login`).

Avec Docker (après avoir copié le JAR sous le nom `joodi-backend.jar` à la racine) :

```bash
docker build -t joodi-backend .
docker run -p 8080:8080 --env-file .env joodi-backend
```

## Configuration

Aucun fichier `.env.example` n'est fourni. Les variables d'environnement lues sont :

- Base de données : `DATABASE_URL`, `DATABASE_USER`, `DATABASE_PASSWORD`, `DATABASE_SCHEMA`
- SMTP : `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASSWORD`
- Sécurité : `JWT_SECRET`
- AWS : `S3_BUCKET`, `S3_REGION`, `S3_ACCESS_KEY`, `S3_SECRET_KEY`,
  `CLOUDFRONT_DISTRIBUTION_ID`, `CLOUDFRONT_KEY_PAIR`, `CLOUDFRONT_PRIVATE_KEY`
- Divers : `APPS_URL`

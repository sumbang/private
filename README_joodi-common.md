# Joodi Common

Bibliothèque Java partagée de la plateforme **Joodi** (podcasts africains). Elle regroupe le
modèle de données, l'accès à la base, la sécurité JWT et les services transverses utilisés par
les deux applications serveur de Joodi :

- `joodi-api` : API REST consommée par les applications mobiles et la passerelle de paiement ;
- `joodi-backend` : console d'administration web.

Public visé : les développeurs des services Joodi.

## Fonctionnalités principales

- **Entités JPA** (`dao`) : comptes et personnes, rôles, podcasts et épisodes, catégories,
  langues, pays, domaines, publications, sliders, abonnements et souscriptions, achats,
  paiements, codes promo, lectures, téléchargements, notes, listes personnelles,
  notifications, exports, ainsi que des vues de statistiques (lectures, téléchargements).
- **Dépôts Spring Data JPA** (`repositories`) pour chacune de ces entités.
- **Sécurité** (`config`) : génération et validation de jetons JWT (`JwtUtils`), filtre
  d'authentification (`AuthTokenFilter`), chargement des utilisateurs (`LoginService`).
- **Services transverses** (`services`) :
  - `AmazonService` : envoi et suppression de fichiers sur Amazon S3, génération de liens
    signés CloudFront ;
  - `EmailService` : envoi d'e-mails via SMTP ;
  - `AbstractService` : base générique des services CRUD.
- **Utilitaires** (`Setting`) : constantes (préfixe et durée des jetons), génération de jetons
  et chaînes aléatoires, validation de mot de passe.
- **Objets d'échange** (`bean`) : réponses génériques, requêtes de réinitialisation de mot de
  passe, jetons de paiement/activation, métriques de contenus, tendances.

## Stack technique

- Java 17
- Spring Boot 3.5 (Data JPA, Security, Validation, Mail, Web Services)
- PostgreSQL
- JJWT (jetons JWT), Lombok
- AWS SDK v2 (S3, CloudFront), Bouncy Castle
- Tests : JUnit 5, Mockito, Testcontainers ; couverture JaCoCo
- Build : Maven (wrapper fourni)
- CI/CD : GitHub Actions (build, publication du JAR en snapshot puis promotion en release
  sur un dépôt d'artefacts Maven)

## Structure du projet

```
src/main/java/africa/joodi/common/
├── bean/           # DTO et objets utilitaires
├── config/         # JWT, filtre d'authentification, source de données, constantes
├── dao/            # entités JPA
├── repositories/   # dépôts Spring Data
├── services/       # S3/CloudFront, e-mail, login, service générique
└── exception/      # exceptions métier
src/main/resources/application.properties   # propriétés attendues (valeurs injectées)
src/test/                                   # tests unitaires
```

## Installation et utilisation

Prérequis : JDK 17 (le wrapper Maven est inclus).

```bash
./mvnw clean install
```

La bibliothèque est alors installée dans le dépôt Maven local et peut être déclarée comme
dépendance :

```xml
<dependency>
    <groupId>africa.joodi</groupId>
    <artifactId>joodi-common</artifactId>
    <version>2.0.1</version>
</dependency>
```

## Configuration

Aucun fichier `.env.example` n'est fourni. Les propriétés de `application.properties` sont
résolues à partir de propriétés fournies par l'application hôte :

- Base de données : `app.database.url`, `app.database.user`, `app.database.pass`,
  `app.database.schema`
- SMTP : `app.mail.host`, `app.mail.port`, `app.mail.username`, `app.mail.password`
- JWT : `app.jwtutil.secret`
- AWS : `aws.s3.bucket-name`, `aws.s3.region`, `aws.s3.access-key`, `aws.s3.secret`,
  `aws.cloudfront.distribution`, `aws.cloudfront.keypairId`, `aws.cloudfront.privateKey`
- URLs : `local-url`, `apps-url`, `img-url`, `bill-url`

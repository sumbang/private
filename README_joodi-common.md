# Joodi Common

Shared Java library for the **Joodi** platform (African podcasts). It contains the
data model, database access, JWT security and cross-cutting services used by
Joodi's two server applications:

- `joodi-api`: REST API used by the mobile apps and the payment gateway;
- `joodi-backend`: web admin console.

Intended audience: developers of the Joodi services.

## Main features

- **JPA entities** (`dao`): accounts and persons, roles, podcasts and episodes, categories,
  languages, countries, domains, publications, sliders, subscription plans and subscriptions,
  purchases, payments, promo codes, plays, downloads, ratings, personal lists,
  notifications, exports, as well as statistics views (plays, downloads).
- **Spring Data JPA repositories** (`repositories`) for each of these entities.
- **Security** (`config`): JWT token generation and validation (`JwtUtils`),
  authentication filter (`AuthTokenFilter`), user loading (`LoginService`).
- **Cross-cutting services** (`services`):
  - `AmazonService`: uploading and deleting files on Amazon S3, generating CloudFront
    signed URLs;
  - `EmailService`: sending emails via SMTP;
  - `AbstractService`: generic base for CRUD services.
- **Utilities** (`Setting`): constants (token prefix and lifetime), generation of random
  tokens and strings, password validation.
- **Data transfer objects** (`bean`): generic responses, password reset requests,
  payment/activation tokens, content metrics, trends.

## Tech stack

- Java 17
- Spring Boot 3.5 (Data JPA, Security, Validation, Mail, Web Services)
- PostgreSQL
- JJWT (JWT tokens), Lombok
- AWS SDK v2 (S3, CloudFront), Bouncy Castle
- Tests: JUnit 5, Mockito, Testcontainers; JaCoCo coverage
- Build: Maven (wrapper included)
- CI/CD: GitHub Actions (build, publishing the JAR as a snapshot, then promotion to a release
  on a Maven artifact repository)

## Project structure

```
src/main/java/africa/joodi/common/
├── bean/           # DTOs and utility objects
├── config/         # JWT, authentication filter, data source, constants
├── dao/            # JPA entities
├── repositories/   # Spring Data repositories
├── services/       # S3/CloudFront, email, login, generic service
└── exception/      # business exceptions
src/main/resources/application.properties   # expected properties (injected values)
src/test/                                   # unit tests
```

## Installation and usage

Prerequisites: JDK 17 (the Maven wrapper is included).

```bash
./mvnw clean install
```

The library is then installed in the local Maven repository and can be declared as a
dependency:

```xml
<dependency>
    <groupId>africa.joodi</groupId>
    <artifactId>joodi-common</artifactId>
    <version>2.0.1</version>
</dependency>
```

## Configuration

No `.env.example` file is provided. The properties in `application.properties` are
resolved from properties supplied by the host application:

- Database: `app.database.url`, `app.database.user`, `app.database.pass`,
  `app.database.schema`
- SMTP: `app.mail.host`, `app.mail.port`, `app.mail.username`, `app.mail.password`
- JWT: `app.jwtutil.secret`
- AWS: `aws.s3.bucket-name`, `aws.s3.region`, `aws.s3.access-key`, `aws.s3.secret`,
  `aws.cloudfront.distribution`, `aws.cloudfront.keypairId`, `aws.cloudfront.privateKey`
- URLs: `local-url`, `apps-url`, `img-url`, `bill-url`

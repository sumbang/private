# Joodi API

REST API for the **Joodi** platform (African podcasts). It serves the Joodi mobile
apps (audio catalog, playback, accounts, subscriptions, purchases) and the web payment
gateway, which calls it to retrieve and close payments.

Intended audience: developers of the Joodi client apps and the operations team.

## Main features

**v1** API (`/api/v1/...`, used by the apps):

- **Authentication** (`/auth`): sign-up, sign-in (JWT token), password reset and
  change, public reference data (languages, countries, domains).
- **Account** (`/account`): activation by code, token verification, profile, update,
  account deletion, sign-out, notifications.
- **Audio** (`/audio`): home page content by section, slider, categories, search,
  podcast and episode details, recording of plays and downloads,
  ratings and "likes".
- **Payment** (`/paiement`): subscription plans, initialization of a subscription or an
  order, promo code validation, history of purchases, subscriptions and payments.

**v0** API (`/api/v0/...`):

- **Audio**: trends, store, content details, plays.
- **Payment**: payment lookup, update of its reference, and closing
  (used by the `joodi_paiement` gateway).

Non-public routes are protected by JWT (Spring Security).

## Tech stack

- Java 17, Spring Boot 3.5 (Web/Data REST, Security)
- Shared library `africa.joodi:joodi-common` 2.0.1 (JPA entities, security, S3, email)
- PostgreSQL (via joodi-common), OkHttp, org.json, Lombok
- Tests: Spring Boot Test / JUnit 5; JaCoCo coverage
- Build: Maven (wrapper included); Docker image based on OpenJDK 17
- CI/CD: GitHub Actions (build, tests, Docker image, publishing to an artifact repository)

## Project structure

```
src/main/java/africa/joodi/api/
├── JoodiApiApplication.java
├── config/            # security, PayPal access token
├── v0/controllers/    # v0 API (audio, payment for the gateway)
└── v1/controllers/    # v1 API (auth, account, audio, payment)
    v1/models/         # JSON requests and responses
src/main/resources/
├── application.properties          # main configuration
└── application-common.properties   # properties passed to joodi-common
src/test/                           # controller tests
Dockerfile, settings.xml            # runtime image, Maven configuration for the artifact repository
```

## Installation and running

Prerequisites: JDK 17, access to the `joodi-common` dependency (installed locally with
`./mvnw install` in the `joodi-common` repository, or from the artifact repository), a
PostgreSQL database.

```bash
./mvnw clean package
java -jar target/joodi-api-*.jar
```

or, for development:

```bash
./mvnw spring-boot:run
```

The service listens on port `8080`.

With Docker (after copying the JAR as `joodi-api.jar` to the project root):

```bash
docker build -t joodi-api .
docker run -p 8080:8080 --env-file .env joodi-api
```

## Configuration

No `.env.example` file is provided. The environment variables read are:

- Database: `DATABASE_URL`, `DATABASE_USER`, `DATABASE_PASSWORD`, `DATABASE_SCHEMA`
- SMTP: `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASSWORD`
- Security: `JWT_SECRET`
- AWS: `S3_BUCKET`, `S3_REGION`, `S3_ACCESS_KEY`, `S3_SECRET_KEY`,
  `CLOUDFRONT_DISTRIBUTION_ID`, `CLOUDFRONT_KEY_PAIR`, `CLOUDFRONT_PRIVATE_KEY`
- Payment: `PAYPAL_TOKEN`, `LYGOS_TOKEN`
- Miscellaneous: `APPS_URL`, `BILL_URL`, `SERVEUR` (target environment)

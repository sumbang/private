# Joodi Backend

Web admin console for the **Joodi** platform (African podcasts). It lets the Joodi team
manage the catalog (podcasts, episodes, categories, publications), users and commercial
offers, and monitor platform activity.

Intended audience: Joodi administrators and operators.

## Main features

- **Form-based authentication** (Spring Security) with roles; account management and
  sending notifications are restricted to the `Administrateur` role. Password reset
  by email.
- **Catalog**: podcasts and their episodes (audio files and images uploaded to
  Amazon S3), categories, languages, publications by platform and region,
  slider banners.
- **Users**: accounts (creation, editing, profile, import and export), persons.
- **Offers and sales**: subscription plans, promo codes, subscriptions, purchases, payments
  (paginated lists and multi-criteria search).
- **Notifications**: sending notifications to app users.
- **Dashboard and statistics** (`exchange`): users, revenue, codes, subscriptions,
  purchases, payments, plays and downloads.
- **Exports and imports** (`export`): extraction of the same data in Excel/CSV format;
  bulk import of users, subscriptions and purchases from an Excel file.

## Tech stack

- Java 17, Spring Boot 3.5 (Web, Data JPA, Data REST, Security, Mail)
- Thymeleaf (layout dialect, Spring Security extras); AdminLTE theme, Bootstrap, jQuery
- Shared library `africa.joodi:joodi-common` 2.0.1 (entities, repositories, S3, email)
- PostgreSQL (via joodi-common)
- Apache POI and Commons CSV (exports), Lombok
- Tests: Spring Boot Test / JUnit 5; JaCoCo coverage
- Build: Maven (wrapper included); Docker image based on OpenJDK 17
- CI/CD: GitHub Actions (build, tests, Docker image, publishing to an artifact repository)

## Project structure

```
src/main/java/africa/joodi/backend/
├── JoodiBackendApplication.java
├── config/        # security configuration
├── controllers/   # MVC controllers (podcast, account, subscription, export, exchange…)
├── services/      # business logic and Excel helper
└── search/        # search criteria for lists
src/main/resources/
├── templates/     # Thymeleaf views by module (podcast/, compte/, dashboard/…)
├── static/        # AdminLTE, Bootstrap and JS plugin assets
├── application.properties
└── application-common.properties
src/test/          # unit tests
```

## Installation and running

Prerequisites: JDK 17, access to the `joodi-common` dependency (installed locally with
`./mvnw install` in the `joodi-common` repository, or from the artifact repository), a
PostgreSQL database.

```bash
./mvnw clean package
java -jar target/joodi-backend-*.jar
```

or, for development:

```bash
./mvnw spring-boot:run
```

The interface is served on port `8080` (login page: `/login`).

With Docker (after copying the JAR as `joodi-backend.jar` to the project root):

```bash
docker build -t joodi-backend .
docker run -p 8080:8080 --env-file .env joodi-backend
```

## Configuration

No `.env.example` file is provided. The environment variables read are:

- Database: `DATABASE_URL`, `DATABASE_USER`, `DATABASE_PASSWORD`, `DATABASE_SCHEMA`
- SMTP: `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASSWORD`
- Security: `JWT_SECRET`
- AWS: `S3_BUCKET`, `S3_REGION`, `S3_ACCESS_KEY`, `S3_SECRET_KEY`,
  `CLOUDFRONT_DISTRIBUTION_ID`, `CLOUDFRONT_KEY_PAIR`, `CLOUDFRONT_PRIVATE_KEY`
- Miscellaneous: `APPS_URL`

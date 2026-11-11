# SYGEM – Mutual insurance management system

Web application (members' portal) for a company mutual insurance fund. It lets members view their financial standing and history with the fund, and submit loan and financial aid requests online.

## Main features

- **User account**: member sign-up (using their employee ID number) with e-mail activation, login, logout, password reset and change
- Member **dashboard**
- Member's **financial standing** and **profile**
- Transaction **history**, with filtering by period and PDF export
- **Loans**: paginated list, search, details, new request, cancellation; simulation of the net amount (management fees and interest depending on the loan type) and buyback calculation through a small JSON API (`/api/typepret`, `/api/calcul`, `/api/rachat`)
- **Financial aid**: list, search, details, new request, cancellation
- Security with Spring Security (access-denied pages, 404 / 500 errors)
- E-mail sending (activation, password reset)

## Tech stack

- **Java 11**
- **Spring Boot 2.7.6**: Web, Data JPA, Data REST, Security, Validation, Mail, Web Services
- **Thymeleaf** (+ Layout Dialect, Spring Security 5 extras), AdminLTE theme, Bootstrap 5, jQuery (WebJars)
- **MySQL** (`mysql-connector-j`), Hibernate
- Lombok, OpenPDF (PDF export), Apache POI, JJWT
- Build: **Maven** (`mvnw` wrapper included)

## Project structure

```
src/main/java/com/adc/sygem/
├── config/         # Constants and settings (security, e-mail)
├── controllers/    # MVC controllers: loans, aid, accounts, settings, JSON API
├── forms/          # Form objects
├── models/         # JPA entities (member, beneficiary, loan, installment, contribution, aid, transaction, role…)
├── repositories/   # Spring Data JPA repositories
├── security/       # Spring Security configuration, login
├── services/       # Business services, e-mail sending, PDF export of the history
└── SygemApplication.java
src/main/resources/
├── application.properties
├── static/         # AdminLTE resources (CSS, JS, images)
└── templates/      # Thymeleaf views (dashboard, pret, aide, user, setting, login, errors)
src/test/           # Spring Boot startup test
```

## Installation and running

Prerequisites: JDK 11, an accessible MySQL database.

```bash
./mvnw clean package
./mvnw spring-boot:run
# or
java -jar target/sygem-0.0.1-SNAPSHOT.jar
```

By default, the application listens on port `8083` with the `/sygem` context path (`http://localhost:8083/sygem`). The schema is updated automatically by Hibernate (`ddl-auto=update`).

Tests:

```bash
./mvnw test
```

## Configuration

There is no `.env.example` file; configuration is done in `src/main/resources/application.properties` (or through the equivalent Spring Boot environment variables / arguments). Fill in your own values for:

- `spring.datasource.url`, `spring.datasource.username`, `spring.datasource.password`: MySQL connection
- `spring.mail.host`, `spring.mail.port`, `spring.mail.username`, `spring.mail.password`: SMTP server
- `server.port`, `server.servlet.context-path`: application port and path
- `spring.servlet.multipart.*`: size and folder for uploaded files

# Dauphine: school management and tuition payment backend

A **Spring Boot** backend application for managing a school. It covers students, classes (rooms), school years (fiscal years) and tuition fees payable in installments. It provides:

- a **web administration interface** (Thymeleaf), protected by Spring Security;
- a **REST API v1** secured with **JWT**, intended for a mobile app for parents. Parents can create an account, link their children, view fees, and make or track payments.

Intended audience: the school administration (back office) and parents (mobile app consuming the API).

## Main features

### Web back office
- Login and logout, access-denied page, **dashboard**.
- **Students**: paginated list, creation, editing, deletion. A record contains the student ID number, identity, date and place of birth, and the father's and mother's contact details.
- **Excel import and export** of students (Apache POI, `.xlsx` format).
- **Rooms** (classes) and **fiscal years** (school years).
- **Assignments**: linking a student to a room for a given school year.
- Tuition **fees** per room and per school year, split into **installments** with an amount per installment.
- **Users**: account management, viewing each user's linked children and payments.

### REST API v1 (`/api/v1`)
- **Authentication** (`/api/v1/auth`): `signup`, `signin`, `signout`, `reset-pwd`, `change-pwd`, `check-token` and `current-year` (current school year).
- **Students** (`/api/v1/eleves`): link a child to the parent's account, list their children, remove a link.
- **Payments** (`/api/v1/paiement`): view fees (`frais`), list payments (`liste`), initialize a payment (`init`), update it (`update`), delete it (`delete`) and view `notifications`.

## Tech stack

- **Java** 8 (`sourceCompatibility = 1.8`)
- **Spring Boot** 2.7.3: Web, Web Services, Data JPA, Data REST, Validation, Security, Mail, DevTools
- **Thymeleaf** with the Layout Dialect and `thymeleaf-extras-springsecurity5`
- **Bootstrap** 5.1.3 and **jQuery** 3.5.1 (WebJars)
- **MySQL** (connector 8.0.30, Hibernate with the MySQL 5 dialect)
- **JWT** with `jjwt` 0.11.2
- **Apache POI** 5.2.2 for Excel import and export
- **Lombok**
- **Gradle** for the build. Tests use JUnit 5 through `spring-boot-starter-test`.

## Project structure

```
src/main/java/tv/wouri/dauphine/
  DauphineApplication.java   Spring Boot entry point
  controllers/               Back-office MVC controllers (students, rooms, school years, fees, installments, assignments, users)
  models/                    JPA entities (Eleve, Salle, Excercise, Frais, Tranche, Paiement, Affectation, User, Role…)
  repositories/              Spring Data JPA repositories
  service/                   Business services
  security/                  Spring Security configuration and back-office login
  config/                    Utilities (Excel import/export, upload error handling)
  apiV1/
    controller/              REST controllers (auth, students, payments)
    config/                  JWT (utilities, filter, constants)
    model/                   API request and response objects
src/main/resources/
  application.properties     Application configuration
  templates/                 Thymeleaf views
  static/                    Static resources
src/test/                    Tests
```

## Installation and running

Prerequisites: JDK 8 or later and a MySQL database.

1. Create a MySQL database and set the connection in `src/main/resources/application.properties`. The schema is created or updated automatically thanks to `spring.jpa.hibernate.ddl-auto=update`.
2. Start the application:
   ```bash
   ./gradlew bootRun
   ```
   The application is served under the `/dauphine` context, for example `http://localhost:8080/dauphine/`.
3. Build the executable JAR:
   ```bash
   ./gradlew build
   java -jar build/libs/dauphine-0.0.1-SNAPSHOT.jar
   ```
4. Run the tests:
   ```bash
   ./gradlew test
   ```

## Configuration

The repository does not include a `.env.example` file. Settings are in `src/main/resources/application.properties`. The main keys are:

- data source: `spring.datasource.url`, `spring.datasource.username`, `spring.datasource.driver-class-name`;
- JPA and Hibernate: `spring.jpa.hibernate.dialect`, `spring.jpa.hibernate.ddl-auto`, `spring.jpa.show-sql`;
- server: `server.servlet.context-path`;
- file uploads: `spring.servlet.multipart.max-file-size`, `spring.servlet.multipart.max-request-size`, `spring.servlet.multipart.location`;
- Thymeleaf and static resources: `spring.thymeleaf.*`, `spring.web.resources.*`.

Do not commit passwords or JWT or SMTP secrets. Prefer environment variables, for example `SPRING_DATASOURCE_PASSWORD`.

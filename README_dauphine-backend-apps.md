# Dauphine : backend de gestion scolaire et de paiement des frais de scolarité

Application backend **Spring Boot** pour la gestion d'un établissement scolaire. Elle couvre les élèves, les classes (salles), les années scolaires (exercices) et les frais de scolarité payables par tranches. Elle propose :

- une **interface d'administration web** (Thymeleaf), protégée par Spring Security ;
- une **API REST v1** sécurisée par **JWT**, destinée à une application mobile pour les parents. Les parents peuvent créer un compte, rattacher leurs enfants, consulter les frais et effectuer ou suivre des paiements.

Public visé : l'administration de l'établissement (back-office) et les parents d'élèves (application mobile cliente de l'API).

## Fonctionnalités principales

### Back-office web
- Connexion et déconnexion, page d'accès refusé, **tableau de bord**.
- **Élèves** : liste paginée, création, modification, suppression. Une fiche contient le matricule, l'identité, la date et le lieu de naissance, ainsi que les coordonnées du père et de la mère.
- **Import et export Excel** des élèves (Apache POI, format `.xlsx`).
- **Salles** (classes) et **exercices** (années scolaires).
- **Affectations** : rattachement d'un élève à une salle pour un exercice donné.
- **Frais** de scolarité par salle et par exercice, découpés en **tranches** avec un montant par tranche.
- **Utilisateurs** : gestion des comptes, consultation des enfants rattachés et des paiements de chaque utilisateur.

### API REST v1 (`/api/v1`)
- **Authentification** (`/api/v1/auth`) : `signup`, `signin`, `signout`, `reset-pwd`, `change-pwd`, `check-token` et `current-year` (exercice en cours).
- **Élèves** (`/api/v1/eleves`) : rattacher un enfant au compte du parent, lister ses enfants, supprimer un rattachement.
- **Paiements** (`/api/v1/paiement`) : consulter les frais (`frais`), lister les paiements (`liste`), initialiser un paiement (`init`), le mettre à jour (`update`), le supprimer (`delete`) et consulter les `notifications`.

## Stack technique

- **Java** 8 (`sourceCompatibility = 1.8`)
- **Spring Boot** 2.7.3 : Web, Web Services, Data JPA, Data REST, Validation, Security, Mail, DevTools
- **Thymeleaf** avec le Layout Dialect et `thymeleaf-extras-springsecurity5`
- **Bootstrap** 5.1.3 et **jQuery** 3.5.1 (WebJars)
- **MySQL** (connecteur 8.0.30, Hibernate en dialecte MySQL 5)
- **JWT** avec `jjwt` 0.11.2
- **Apache POI** 5.2.2 pour l'import et l'export Excel
- **Lombok**
- **Gradle** pour le build. Les tests utilisent JUnit 5 via `spring-boot-starter-test`.

## Structure du projet

```
src/main/java/tv/wouri/dauphine/
  DauphineApplication.java   Point d'entrée Spring Boot
  controllers/               Contrôleurs MVC du back-office (élèves, salles, exercices, frais, tranches, affectations, utilisateurs)
  models/                    Entités JPA (Eleve, Salle, Excercise, Frais, Tranche, Paiement, Affectation, User, Role…)
  repositories/              Repositories Spring Data JPA
  service/                   Services métier
  security/                  Configuration Spring Security et connexion du back-office
  config/                    Utilitaires (import/export Excel, gestion des erreurs d'upload)
  apiV1/
    controller/              Contrôleurs REST (auth, élèves, paiements)
    config/                  JWT (utilitaires, filtre, constantes)
    model/                   Objets de requête et de réponse de l'API
src/main/resources/
  application.properties     Configuration de l'application
  templates/                 Vues Thymeleaf
  static/                    Ressources statiques
src/test/                    Tests
```

## Installation et lancement

Prérequis : un JDK 8 ou plus récent et une base de données MySQL.

1. Créer une base MySQL et renseigner la connexion dans `src/main/resources/application.properties`. Le schéma est créé ou mis à jour automatiquement grâce à `spring.jpa.hibernate.ddl-auto=update`.
2. Lancer l'application :
   ```bash
   ./gradlew bootRun
   ```
   L'application est servie sous le contexte `/dauphine`, par exemple `http://localhost:8080/dauphine/`.
3. Construire le JAR exécutable :
   ```bash
   ./gradlew build
   java -jar build/libs/dauphine-0.0.1-SNAPSHOT.jar
   ```
4. Lancer les tests :
   ```bash
   ./gradlew test
   ```

## Configuration

Le dépôt ne contient pas de fichier `.env.example`. Les réglages se trouvent dans `src/main/resources/application.properties`. Les principales clés sont :

- source de données : `spring.datasource.url`, `spring.datasource.username`, `spring.datasource.driver-class-name` ;
- JPA et Hibernate : `spring.jpa.hibernate.dialect`, `spring.jpa.hibernate.ddl-auto`, `spring.jpa.show-sql` ;
- serveur : `server.servlet.context-path` ;
- téléversement de fichiers : `spring.servlet.multipart.max-file-size`, `spring.servlet.multipart.max-request-size`, `spring.servlet.multipart.location` ;
- Thymeleaf et ressources statiques : `spring.thymeleaf.*`, `spring.web.resources.*`.

Ne versionnez pas de mots de passe ni de secrets JWT ou SMTP. Préférez des variables d'environnement, par exemple `SPRING_DATASOURCE_PASSWORD`.

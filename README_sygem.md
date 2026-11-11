# SYGEM – Système de gestion de la mutuelle

Application web (portail des mutualistes) d'une mutuelle d'entreprise. Elle permet aux adhérents de consulter leur situation financière et leur historique auprès de la mutuelle, et de soumettre en ligne leurs demandes de prêt et d'aide financière.

## Fonctionnalités principales

- **Compte utilisateur** : inscription d'un mutualiste (à partir de son matricule) avec activation par e-mail, connexion, déconnexion, réinitialisation et changement du mot de passe
- **Tableau de bord** de l'adhérent
- **Situation financière** et **profil** du mutualiste
- **Historique** des opérations, avec filtrage par période et export PDF
- **Prêts** : liste paginée, recherche, détail, nouvelle demande, annulation ; simulation du montant net (frais de gestion et intérêts selon le type de prêt) et calcul de rachat via une petite API JSON (`/api/typepret`, `/api/calcul`, `/api/rachat`)
- **Aides financières** : liste, recherche, détail, nouvelle demande, annulation
- Sécurité par Spring Security (pages d'accès refusé, erreurs 404 / 500)
- Envoi d'e-mails (activation, réinitialisation du mot de passe)

## Stack technique

- **Java 11**
- **Spring Boot 2.7.6** : Web, Data JPA, Data REST, Security, Validation, Mail, Web Services
- **Thymeleaf** (+ Layout Dialect, extras Spring Security 5), thème AdminLTE, Bootstrap 5, jQuery (WebJars)
- **MySQL** (`mysql-connector-j`), Hibernate
- Lombok, OpenPDF (export PDF), Apache POI, JJWT
- Build : **Maven** (wrapper `mvnw` fourni)

## Structure du projet

```
src/main/java/com/adc/sygem/
├── config/         # Constantes et paramètres (sécurité, e-mail)
├── controllers/    # Contrôleurs MVC : prêts, aides, comptes, paramètres, API JSON
├── forms/          # Objets de formulaire
├── models/         # Entités JPA (mutualiste, ayant droit, prêt, échéance, cotisation, aide, transaction, rôle…)
├── repositories/   # Dépôts Spring Data JPA
├── security/       # Configuration Spring Security, connexion
├── services/       # Services métier, envoi d'e-mails, export PDF de l'historique
└── SygemApplication.java
src/main/resources/
├── application.properties
├── static/         # Ressources AdminLTE (CSS, JS, images)
└── templates/      # Vues Thymeleaf (dashboard, pret, aide, user, setting, login, erreurs)
src/test/           # Test de démarrage Spring Boot
```

## Installation et lancement

Prérequis : JDK 11, une base MySQL accessible.

```bash
./mvnw clean package
./mvnw spring-boot:run
# ou
java -jar target/sygem-0.0.1-SNAPSHOT.jar
```

Par défaut, l'application écoute sur le port `8083` avec le chemin de contexte `/sygem` (`http://localhost:8083/sygem`). Le schéma est mis à jour automatiquement par Hibernate (`ddl-auto=update`).

Tests :

```bash
./mvnw test
```

## Configuration

Il n'y a pas de fichier `.env.example` ; la configuration se fait dans `src/main/resources/application.properties` (ou via les variables d'environnement / arguments Spring Boot équivalents). Renseignez vos propres valeurs pour :

- `spring.datasource.url`, `spring.datasource.username`, `spring.datasource.password` : connexion MySQL
- `spring.mail.host`, `spring.mail.port`, `spring.mail.username`, `spring.mail.password` : serveur SMTP
- `server.port`, `server.servlet.context-path` : port et chemin de l'application
- `spring.servlet.multipart.*` : taille et dossier des fichiers téléversés

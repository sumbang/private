# Royal Onyx (royal-mob) : application mobile Android de souscription et de suivi d'assurance

Application Android native (Java) « Royal Onyx » pour les commerciaux et les clients d'une compagnie d'assurance. Elle permet de se connecter à la plateforme, de créer et de suivre des **propositions d'assurance AUTO et IAC**, de consulter les **contrats** correspondants et de gérer un portefeuille de **prospects**.

Public visé : les agents ou commerciaux qui saisissent des propositions et suivent leurs prospects, et les clients qui consultent leurs contrats. Les menus et l'écran d'accueil changent selon le rôle de l'utilisateur connecté.

## Fonctionnalités principales

- **Connexion** par identifiant et mot de passe. La session et le rôle sont conservés dans les `SharedPreferences`.
- **Tableau de bord** dont le contenu dépend du profil de l'utilisateur.
- **Propositions AUTO** : liste, détail et création d'une proposition.
- **Propositions IAC** : liste, détail et création d'une proposition.
- **Contrats AUTO et IAC** : liste des contrats (numéro, nom, montant, statut) pour les profils concernés.
- **Prospects** : liste et mise à jour des fiches (nom, prénom, e-mail, téléphone, code client, profession, adresse).
- **Mon compte** : consultation du compte de l'utilisateur.
- **Déconnexion**.
- Les formulaires de création et de détail s'affichent dans des **WebView** qui chargent les pages de la plateforme web. Ces écrans permettent de joindre des fichiers ou des photos (sélecteur de fichiers, appareil photo).

## Stack technique

- **Java**, application Android native
- **Gradle** (Android Gradle Plugin), `compileSdkVersion` / `targetSdkVersion` 29, `minSdkVersion` 21
- **AndroidX** : AppCompat, ConstraintLayout, DrawerLayout, CoordinatorLayout
- **Material Components** 1.0.0 (navigation latérale)
- **OkHttp** 3.10 pour les appels HTTP au backend
- Tests : JUnit 4, AndroidX Test et Espresso

## Structure du projet

```
app/
  build.gradle                         Configuration du module application
  src/main/AndroidManifest.xml         Déclaration des activités et permissions
  src/main/java/cm/royalonyx/apps/     Code source
    MainActivity.java                  Écran de connexion
    HomeActivity.java                  Tableau de bord et navigation
    ListAutoActivity / AddAutoActivity / AutoDetailActivity   Propositions et contrats AUTO
    ListIacActivity / AddIacActivity / IacDetailActivity      Propositions et contrats IAC
    ListProspectActivity / UpdateProspectActivity            Prospects
    AccountActivity.java               Mon compte
    HttpQuery.java                     Client HTTP (OkHttp)
    Contrat, Prospect (+ Adapters)     Modèles et adaptateurs de listes
  src/main/res/                        Layouts, menus, ressources
build.gradle, settings.gradle          Configuration Gradle du projet
```

## Installation et lancement

Prérequis : Android Studio, ou le SDK Android avec un JDK compatible avec la version Gradle du projet.

1. Ouvrir le dossier du projet dans Android Studio et laisser la synchronisation Gradle se faire.
2. Compiler un APK de debug :
   ```bash
   ./gradlew assembleDebug
   ```
3. Installer l'application sur un appareil ou un émulateur connecté :
   ```bash
   ./gradlew installDebug
   ```
4. Lancer les tests unitaires :
   ```bash
   ./gradlew test
   ```

## Configuration

Le projet ne contient pas de fichier `.env.example`. L'adresse du backend est définie dans le code source (`HttpQuery.java` et les activités qui utilisent une WebView). Adaptez-la à votre environnement avant de compiler.

Permissions Android demandées : Internet, état du réseau et du Wi-Fi, appareil photo, lecture et écriture du stockage externe, état du téléphone, démarrage au boot et wake lock.

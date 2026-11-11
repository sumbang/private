# Royal Onyx (royal-mob): Android mobile app for insurance subscription and tracking

Native Android (Java) "Royal Onyx" app for the sales agents and customers of an insurance company. It lets users sign in to the platform, create and track **AUTO and IAC insurance proposals**, view the corresponding **contracts**, and manage a portfolio of **prospects**.

Intended audience: agents or sales representatives who enter proposals and follow up on their prospects, and customers who view their contracts. The menus and the home screen change depending on the role of the signed-in user.

## Main features

- **Sign-in** with username and password. The session and role are stored in `SharedPreferences`.
- **Dashboard** whose content depends on the user's profile.
- **AUTO proposals**: list, details and creation of a proposal.
- **IAC proposals**: list, details and creation of a proposal.
- **AUTO and IAC contracts**: list of contracts (number, name, amount, status) for the relevant profiles.
- **Prospects**: list and update of records (last name, first name, email, phone, customer code, occupation, address).
- **My account**: view of the user's account.
- **Sign-out**.
- The creation and detail forms are displayed in **WebViews** that load pages from the web platform. These screens allow attaching files or photos (file picker, camera).

## Tech stack

- **Java**, native Android app
- **Gradle** (Android Gradle Plugin), `compileSdkVersion` / `targetSdkVersion` 29, `minSdkVersion` 21
- **AndroidX**: AppCompat, ConstraintLayout, DrawerLayout, CoordinatorLayout
- **Material Components** 1.0.0 (navigation drawer)
- **OkHttp** 3.10 for HTTP calls to the backend
- Tests: JUnit 4, AndroidX Test and Espresso

## Project structure

```
app/
  build.gradle                         App module configuration
  src/main/AndroidManifest.xml         Activity and permission declarations
  src/main/java/cm/royalonyx/apps/     Source code
    MainActivity.java                  Sign-in screen
    HomeActivity.java                  Dashboard and navigation
    ListAutoActivity / AddAutoActivity / AutoDetailActivity   AUTO proposals and contracts
    ListIacActivity / AddIacActivity / IacDetailActivity      IAC proposals and contracts
    ListProspectActivity / UpdateProspectActivity            Prospects
    AccountActivity.java               My account
    HttpQuery.java                     HTTP client (OkHttp)
    Contrat, Prospect (+ Adapters)     Models and list adapters
  src/main/res/                        Layouts, menus, resources
build.gradle, settings.gradle          Project Gradle configuration
```

## Installation and running

Prerequisites: Android Studio, or the Android SDK with a JDK compatible with the project's Gradle version.

1. Open the project folder in Android Studio and let the Gradle sync complete.
2. Build a debug APK:
   ```bash
   ./gradlew assembleDebug
   ```
3. Install the app on a connected device or emulator:
   ```bash
   ./gradlew installDebug
   ```
4. Run the unit tests:
   ```bash
   ./gradlew test
   ```

## Configuration

The project does not contain a `.env.example` file. The backend address is defined in the source code (`HttpQuery.java` and the activities that use a WebView). Adapt it to your environment before building.

Requested Android permissions: Internet, network and Wi-Fi state, camera, external storage read and write, phone state, start on boot, and wake lock.

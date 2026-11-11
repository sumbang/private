# TSS Performance — desktop app (Electron)

**Electron** shell for the online training app **"Tss Performance, A chacun son tableau de bord"**. It displays the web version of the TSS Performance platform (the Flutter app from the `tsscourses` repository, built for the web) in a native window, and produces installers for **Windows** and **macOS**.

It is aimed at learners who prefer to follow their courses from an app installed on their computer rather than from the browser.

## Features

- Native 1024 × 768 window titled "Tss Performance", with the app icon
- Clears the session cache on startup, then loads the hosted web app (the URL is set in `src/main/main.js`)
- Context isolation enabled and Node integration disabled in the web page
- On Windows and Linux the app quits when all windows are closed; on macOS it stays active and recreates a window when reactivated
- Installer creation: NSIS or packaged executable for Windows, DMG for macOS (`electron-builder` configuration also provided for Linux: AppImage, deb, rpm)

## Tech stack

- **Electron** `^32.0.2` (JavaScript / Node.js)
- Packaging: `electron-builder` `^25`, `electron-packager` `^17`
- Windows installer: `electron-winstaller` `^5` (Squirrel), `electron-installer-windows` `^3`
- Application ID: `mobile.tssperformance.com`

## Project structure

```
.
├── package.json           # Root manifest (Electron + electron-winstaller)
├── create-installer.js    # Builds a Squirrel Windows installer into installer/
├── Squirrel.exe           # Squirrel binary used by the Windows installer
├── windows_version/       # Windows-specific Electron project
│   ├── src/main/main.js   # Main process (window + loading the web app)
│   ├── resources/         # favicon.ico icon
│   ├── config.json        # Installer settings (destination, icon, version)
│   └── package.json       # start / package / build scripts (electron-builder --win)
├── mac_version/           # macOS-specific Electron project
│   ├── src/main/main.js   # Main process
│   ├── resources/         # icon.icns, icon.png, background.png (DMG)
│   ├── config.json
│   └── package.json       # start / build scripts (DMG)
├── resources/app/         # Older app tree (icons, Info.plist, package.json)
└── no_use.txt             # Old package.json kept for reference (unused)
```

## Installation and running

Prerequisites: Node.js and npm.

### Windows

```bash
cd windows_version
npm install
npm start            # run the app in development
npm run build        # NSIS installer via electron-builder (dist/ folder)
npm run package      # packaged executable via electron-packager
```

### macOS

```bash
cd mac_version
npm install
npm start            # run the app in development
npm run build        # DMG via electron-builder (dist/ folder)
```

### Squirrel installer (root)

```bash
npm install
npm run create-installer   # runs create-installer.js, output in installer/
```

The script expects the `tssperformance.exe` executable and the `resources/app/public/icons/favicon.ico` icon in the root folder.

## Configuration

No environment variables are used. To point to another instance of the web app, change the `win.loadURL(...)` call in `windows_version/src/main/main.js` and `mac_version/src/main/main.js`.

## Related project

- `tsscourses`: the TSS Performance Flutter app (mobile, tablet, desktop and web), whose web version is displayed by this Electron shell.

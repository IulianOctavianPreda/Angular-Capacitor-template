# Template - Angular + Capacitor

This template aims to ease the starting of a cross-platform application.
With the release of the [Capacitor](https://capacitor.ionicframework.com/) native plugins you can create cross-platform apps without the need of an additional framework on top of your preferred one, for example Ionic.

## Current versions

- Angular 9.0.3
- Capacitor 2.1.3

## How to use

- Clone the template
- Run `npm run restore` to install the node modules for the web app and the electron app
- Find and replace "TemplateApp" and "template-app" with your preferred name for the project
- Change the Readme
- Use it as your own (It actually is right now) - install the packages you want, delete the platforms you don't like, you can even not use capacitor plugins at all but only wanted a simpler way to build you webapp for different platforms.

## Package.json scripts

The package.json lists all the scripts you might need, you can add more or delete the ones you don't like.

Most script will have the ending

- `:web` for the web project,
- `:android` for the android project
- `:electron` for the electron project
- `:ios` for the ios project

### Scripts:

- `cd` - `cd:web`, `cd:android`, `cd:electron`, `cd:ios` - changes the path from the root to the desired platform folder

- `add` - `add:android`, `add:electron`, `add:ios` - adds the desired platform to the project - it will create a folder with the same name in the root folder - It must be used only once as initialization, and the current template already initialized them and updated the packages to run out of the box, feel free to delete any of the `android`, `electron` or `ios` folders

- `open` - `open:android`, `open:electron`, `open:ios` - opens the project for the desired platform in the software of choice, for android it opens it in Android Studio(it must be installed), for IOS it opens XCode(can be used only on Macs), electron will open the app(think of it as ng serve for an Angular app)

- `copy` - `copy:android`, `copy:electron`, `copy:ios` - copy will copy the distribution folder of the web application to the platforms folder

- `build` - `build:web`, `build:android`, `build:electron`, `build:ios` - all build scripts will build the web, additionally the platform specific ones will also copy the distribution folder of the web application to the platforms folder

- `start` - `start:web`, `start:android`, `start:electron`, `start:ios` - start is a combination of build and open scripts

- `package` - `package:electron` - command available only for electron right now will create a non installable executable for the electron application - it uses a different script from the electron package.json that relies on electron-builder.

## Instructions for adding capacitor to an existing Angular project

- run `ng add @capacitor/angular` or `npm i @capacitor/angular`
- run `npx cap init` or if you chose to copy the scripts from package.json, run `npm run init:capacitor`
- run the `add:<platform>` scripts from package.json where `<platform>` can be android, electron or ios
- check the issues section and make the appropriate changes

### Issues

### Electron

[Security issue](https://github.com/angular/angular/issues/30835) related to how the scripts are loaded.
You need to change:

- In the webapp in `src/index.html` the line `<base href="/">` to `<base href="./">`
- In `electron/index.js` the line `mainWindow.loadURL(`file://\${\_\_dirname}/app/index.html`);` to `mainWindow.loadURL(`\${\_\_dirname}/app/index.html`);`

### Android

For capacitor 2.x everything works out of the box.
For capacitor 1.x you will need to import the Android X libraries and change some imports as they are outdated
You need to add in `android/gradle.properties`

```
    android.useAndroidX=true
    android.enableJetifier=true
```

You need to change in `android/app/src/main`

```
<provider
    android:name="android.support.v4.content.FileProvider"
    android:authorities="${applicationId}.fileprovider"
    android:exported="false"
    android:grantUriPermissions="true">
    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/file_paths">
    </meta-data>
</provider>
```

to

```
<provider
    android:name="androidx.core.content.FileProvider"
    android:authorities="${applicationId}.fileProvider"
    android:exported="false"
    android:grantUriPermissions="true">
    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/file_paths" />
</provider>
```

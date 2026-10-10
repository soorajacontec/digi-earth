# Digi Earth

A calm, living window onto the planet. Digi Earth is an ambient soundscape web app with animated forests, mountains, cities, shores and skies. Set the hour from sunrise to night, visit real places around the world, and listen to ambience that is either generated live or recorded in the field.

Created by **SSK-BLUM**.

<p>
  <img src="screenshots/forest-sunrise.png" width="200" alt="Tropical Rainforest at sunrise">
  <img src="screenshots/city-night.png" width="200" alt="Neon Rain City at night">
  <img src="screenshots/shore-sunset.png" width="200" alt="Tropical Lagoon at sunset">
  <img src="screenshots/sky-aurora.png" width="200" alt="Aurora Skies at night">
</p>

## Features

- **5 worlds, 31 scenes**: Forest, Mountain, City, Sea Shore and Sky, each drawn live on a canvas with weather, wildlife and parallax.
- **Time of day**: Sunrise, Day, Sunset and Night change the light, the sky, the sound mix and the app's light or dark theme.
- **80 real places**, including 11 in Kerala, with region filters. *Match local time* follows each place's real clock.
- **Two sound modes**: *Electronic* generates every sound live with the Web Audio API. *Original* plays real field recordings.
- **Sound mixer** with 8 layers per world, a master volume and a sleep timer that fades the sound out.
- **Background playback**: lock-screen controls with scene artwork, plus a picture-in-picture mini player where supported.
- **Installable PWA** that works offline after the first visit.
- **Android app** built automatically by GitHub, with lock-screen media controls, background playback and picture-in-picture.
- Accent colour picker, a built-in "How to use" guide with spotlights, and support for reduced motion.

## Run it

It is a static site with no build step and no dependencies.

```bash
# from the project folder
python3 -m http.server 8000
# then open http://localhost:8000
```

Opening `index.html` straight from disk mostly works, but the offline service worker and the original recordings need a web server.

## Publish on GitHub Pages

1. Create a new repository on GitHub, for example `digi-earth`.
2. Upload everything in this folder to the repository root (on the GitHub website: **Add file → Upload files**, then drag the files and folders in).
3. Go to **Settings → Pages**, set **Source** to *Deploy from a branch*, choose `main` and `/ (root)`, and save.
4. After a minute the app is live at `https://<your-username>.github.io/digi-earth/`.
5. Open that address on your phone to install it: on Android tap **Install** in the *How to use* panel or Chrome's menu; on iPhone use **Share → Add to Home Screen**.

The empty `.nojekyll` file tells GitHub Pages to serve the files as they are.

## Android app

The `android/` folder wraps the same web app in a native Android app (Android 8 and newer). It bundles every file, so it works fully offline, and adds:

- a media notification on the lock screen with **Pause/Play**, **Next scene** and **Stop**
- sound that keeps playing with the screen locked or while you use other apps
- picture-in-picture: press Home while sound is playing and the live scene shrinks into a floating window
- status bar icons that switch between light and dark with the time of day, and an edge-to-edge scene

### Get the APK (no computer setup needed)

GitHub builds the app for you every time you push to `main` (see `.github/workflows/android.yml`).

1. Open the **Actions** tab of your repository and select the latest **Build Android app** run.
2. When it finishes (about 3–5 minutes), download **DigiEarth-android** under *Artifacts*. It is a zip containing `DigiEarth.apk`.
3. Copy the APK to your phone and open it. Android asks you to allow installs from that app (Files or Chrome) the first time.

To get a direct download link instead, create a release (**Releases → Draft a new release**, tag `v1.0`, publish). The APK is attached to the release automatically.

### Build it yourself

Open the `android` folder in Android Studio and press **Run**, or from a terminal:

```bash
cd android
./gradlew assembleRelease
# APK: android/app/build/outputs/apk/release/
```

The web files from the repository root are copied into the app on every build, so edit `index.html` once and both the website and the app update.

### Publishing on Google Play

Google Play needs the app signed with your own key.

1. Create a key once (keep the file and passwords safe; you need them for every update):
   `keytool -genkeypair -v -keystore digi-earth.jks -alias digiearth -keyalg RSA -keysize 2048 -validity 10000`
2. In **Settings → Secrets and variables → Actions**, add four secrets:
   `KEYSTORE_BASE64` (the output of `base64 -w0 digi-earth.jks`), `KEYSTORE_PASSWORD`, `KEY_ALIAS` (`digiearth`) and `KEY_PASSWORD`.
3. The next build also produces `DigiEarth.aab`, the file you upload to the Play Console.
4. Raise `versionCode` and `versionName` in `android/app/build.gradle` for every new upload.

## Project structure

```
index.html            the whole app: markup, styles and script
manifest.webmanifest  PWA name, colours, icons and screenshots
sw.js                 service worker for offline use
icons/                app and home-screen icons
sounds/               15 looped field recordings (MP3)
fonts/                bundled fonts, so the app works offline
screenshots/          images used in this README and the install dialog
android/              Android app project (WebView wrapper, notification, PiP)
.github/workflows/    automatic Android build
```

When you change `index.html`, also bump `VERSION` in `sw.js` (for example from `digi-earth-v6` to `digi-earth-v7`) so installed copies pick up the update.

## Credits and licences

The code is released under the MIT License (see `LICENSE`).

The sound recordings come from other people and keep their own licences. See [`CREDITS.md`](CREDITS.md) for the full list and attribution.

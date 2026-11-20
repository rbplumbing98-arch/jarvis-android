# J.A.R.V.I.S. for Android

An Android voice assistant with a gold holographic HUD, on-device notes/facts/taught phrases, speech output, media controls, notification access, navigation/dialler shortcuts, and a media-browser service intended for Android Auto discovery.

## Current capability and limits

- **Works without an AI API key:** local command routing, notes, saved facts, taught phrases, cached answers, time, navigation intents, dialler intents, and supported media-session controls.
- **Optional online AI:** open-ended questions are sent to Anthropic only when the device has internet access and an API key is configured. API use may incur charges. This is not an offline large language model.
- **API key storage:** the app encrypts the key with an AES-GCM key held by Android Keystore. Existing plaintext key settings are migrated on first read and removed from the old preferences.
- **Voice recognition:** Android's installed speech recognition provider handles recognition. `EXTRA_PREFER_OFFLINE` requests offline recognition, but availability depends on the device and downloaded language packs.
- **Spotify:** media-session controls depend on Spotify exposing an active media session. Search-to-play may open Spotify and is not guaranteed to start playback silently.
- **Messages:** reading notifications requires the user to grant Notification Access in Android settings. Android Auto itself controls its own messaging experience.
- **Android Auto:** this project registers a media-browser service; this alone does not guarantee approval, visibility, or every advertised action in every Android Auto/head-unit version. Real-device testing is required.

## Build

### Android Studio
1. Install Android Studio with Android SDK Platform 34 and JDK 17.
2. Open this project folder.
3. Allow Gradle to sync.
4. Connect an Android phone with USB debugging enabled, or select Build > Build Bundle(s) / APK(s) > Build APK(s).
5. Install the generated `app/build/outputs/apk/debug/app-debug.apk`.

### GitHub Actions
The repository includes `.github/workflows/build.yml`. Push the project to a GitHub repository and run the workflow to build the debug APK. Download the `jarvis-debug-apk` artifact from the completed workflow.

## First-run setup
1. Open J.A.R.V.I.S. and grant microphone permission when prompted.
2. Open SETTINGS and optionally enter your name, general area, and Anthropic API key. Do not share the key. Online AI requests can incur provider charges.
3. Grant notification access only if you want notification reading and media-session discovery.
4. Enable the optional read-incoming-messages setting if desired.

## Example commands
- `my name is X`, `I live in X`, `learn that <fact>`, `what have you learned`
- `when I say X you say Y`, `forget everything`
- `remember <note>`, `show notes`, `clear notes`
- `play <song or artist>`, `play my usual`, `pause`, `resume`, `next`, `previous`, `what's playing`
- `navigate to <place>`, `navigate home`, `call <number>`
- `read my messages`, `read: <pasted text>`

## Important
This source has not yet been verified on a physical Android device. Before relying on it, build with Android Studio/CI and test microphone permission, speech recognition, text-to-speech, notifications, Spotify, navigation, and Android Auto on the target phone and vehicle. No app can replace the Android Auto wake word or draw an unrestricted custom HUD over the car display.


## Fastest way to get your APK (GitHub Actions)
1. On GitHub, create a **private** repository named `jarvis-android`.
2. On the repository page choose **Add file → Upload files**.
3. Extract this ZIP on your computer and upload the *contents of the `jarvis-android` folder* (not the enclosing folder). Ensure `.github/workflows/build.yml` is included; on GitHub's upload page hidden folders may need to be added from desktop.
4. Commit to the `main` branch. Open the repository's **Actions** tab and select **Build J.A.R.V.I.S. Android APK**.
5. When the run shows a green check, open it and download **jarvis-android-debug-apk** under Artifacts. Extract the downloaded artifact ZIP; install `app-debug.apk` on your Android phone.
6. If Android blocks the install, allow your browser/file manager to install unknown apps for this one installation.

The debug APK is signed with the standard Android debug key and is suitable for testing, not Play Store distribution. Each new build will have the same application ID (`com.jarvis.assistant`), so a later debug build can update the installed app unless its signing identity changes.

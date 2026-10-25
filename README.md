# LifeQuest

LifeQuest is an original, offline-first life quest tracker built as a single-file web app and packaged with Capacitor. There are no accounts, ads, analytics, external fonts, or background tracking. Your data is stored in the app's local storage. The optional AI coach sends a compact summary only when you explicitly press the AI review button.

## Build an APK from a phone or iPad

You do not need Android Studio or a computer. GitHub Actions builds the APK in the cloud.

1. Create a GitHub account if needed, then create a new repository named `LifeQuest`.
2. Upload the contents of this project to the repository root. Make sure `.github/workflows/build-apk.yml` is included. On GitHub mobile, use the website's **Add file → Upload files** workflow; upload the folders and files, not the ZIP as a single file.
3. Open the repository's **Actions** tab and select **Build APK**.
4. If GitHub asks, enable workflows. Tap **Run workflow**. A push to the repository also starts a build.
5. Wait for the run to finish successfully. Open the run and find **Artifacts** near the bottom.
6. Download `LifeQuest-debug-apk` and unzip it. Inside is `app-debug.apk`.
7. On your Android phone, tap `app-debug.apk`. If prompted, allow your browser or file manager to **Install unknown apps**, then return and install. Only install APKs from a repository and workflow you trust.
8. Open LifeQuest. On first launch, choose **Starter pack** if you want the included quest and reward set.

The APK from this workflow is a **debug APK**, suitable for personal installation and testing. It is not a signed Play Store release.

### GitHub Actions troubleshooting

- The first build can take several minutes while Gradle downloads Android dependencies.
- The workflow attempts `npm ci` first and falls back to `npm install` if the lockfile is stale or incomplete. Keep `package.json` and `package-lock.json` together; a successful cloud install refreshes dependency resolution.
- If a build fails, open the failed step's log. A fresh build may be needed after a transient dependency download failure.
- GitHub's web editor on iPad/phone can be awkward with folders. Upload a ZIP's extracted contents using a browser in desktop mode if necessary.

## Fallback: package the web app with PWABuilder

1. Host the `www/` folder as a website, for example with GitHub Pages. In the repository settings, enable **Pages** and select the branch and folder that contains the built web app. If the app is in a subfolder, make sure the Pages URL points to that subfolder.
2. Open `https://www.pwabuilder.com/` and enter the deployed URL.
3. Follow its Android packaging steps. Review the generated package and test it before installing. PWABuilder's options and requirements may change.

This fallback packages a hosted web app and is not identical to the Capacitor build. The hosted site needs to remain available for first-time loads unless the generated package provides suitable caching.

## Optional: signed release build

For personal distribution, create a private Android signing keystore and keep it safe. Never commit a keystore, passwords, or signing secrets to the repository.

1. Create a keystore using Android's `keytool` on a trusted computer or cloud development environment.
2. Store the keystore and passwords in GitHub repository **Settings → Secrets and variables → Actions**.
3. Add a separate release workflow that downloads the keystore from a secret, configures Gradle signing properties through environment variables, and runs `./gradlew assembleRelease`.
4. Keep an encrypted offline backup of the keystore. If it is lost, you may not be able to publish updates under the same signing identity.

The included workflow intentionally creates a debug build and does not sign a release.

## Features

- Five tabs: Quests, Progress, Coach, Rewards, More.
- Local XP, levels, stat progression, coins, daily streaks, achievements, reward redemption and reversible completions.
- Check-ins for mood, energy and weight; JSON export/import; local-only history.
- Inline SVG charts and offline rule-based coaching.
- Optional user-supplied OpenAI or Anthropic API key for a structured AI review.
- Service-worker shell cache for the hosted web app.

## Privacy and limitations

- No account, analytics, ad SDK, remote database, or routine network calls.
- AI calls are optional and made only when you tap the review button. The app sends a compact aggregate summary. Quest titles are included as task labels; enabling “share journal” additionally allows the same quest titles to be included explicitly. The API key is stored locally and is readable by someone with access to the device.
- Local notifications require native Android permissions. On Android 13+, notification permission must be granted.
- The current target SDK is set to API 36 in the workflow to align with the 2026 Android target expectation. Google Play requirements can change; check the current policy before publishing.
- This project is provided as source. A build is only verified after GitHub Actions completes successfully. No APK is bundled with this source archive.

# Dumped to Billionaire

Every push to `main` builds two things (GitHub → **Actions** → latest run → **Artifacts**):

| File | What it is |
|---|---|
| `...-TEST-apk` | Install on your phone for testing. Tester code `TESTER696` works, test ads. |
| `...-GOOGLE-PLAY-aab` | Upload this to Google Play. Tester code is OFF. Real ads once you add your AdMob IDs. |

The app targets **Android 16 (API 36)**, which Google Play requires for new apps and updates.

---

## One-time setup before your first Google Play upload

### 1. Create your upload key (5 minutes)
1. GitHub repo → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**
   - Name: `UPLOAD_KEY_PASSWORD`
   - Value: a strong password (write it down somewhere safe!)
2. Go to **Actions** → **Make upload key (run ONCE)** → **Run workflow**.
3. When it finishes, open the run and download the **upload-key** file. Inside are:
   - `upload.jks` → save it in Google Drive together with your password. **Never lose it.**
   - `UPLOAD_KEYSTORE_B64.txt` → open it, copy ALL the text.
4. Add another secret: Name `UPLOAD_KEYSTORE_B64`, Value = the text you copied.
5. On that run's page, delete the artifact (trash icon).

From now on every build's `GOOGLE-PLAY-aab` is signed and ready to upload.

### 2. Real ads (AdMob)
1. Create an app in AdMob (apps.admob.com), then create a **Rewarded** ad unit.
2. Repo → **Settings** → **Secrets and variables** → **Actions** → **Variables** tab → **New repository variable**:
   - `ADMOB_APP_ID` = your App ID (`ca-app-pub-xxxxxxxx~yyyyyyyy`)
   - `ADMOB_REWARDED_ID` = your Rewarded ad unit ID (`ca-app-pub-xxxxxxxx/yyyyyyyy`)
3. In AdMob → **Privacy & messaging**, create a **GDPR message** (EU/UK consent). The game already shows it automatically.
4. Never tap your own real ads. The TEST APK always uses test ads, so test with that one.

### 3. Google Play Console
1. Create a developer account (one-time fee) at play.google.com/console.
2. **Create app** → name "Dumped to Billionaire", Game, Free.
3. Upload the `GOOGLE-PLAY-aab` to **Testing → Internal testing** first, then Production.
4. Use Google Play App Signing (the default). Your `upload.jks` is only the upload key.
5. Fill in:
   - **Privacy policy URL** (required because of ads)
   - **Ads**: Yes, contains ads
   - **Data safety**: ads collect device/advertising ID
   - **Content rating** questionnaire (cartoon violence: robberies; romance storyline)
   - **Target audience**: 13+ (not for children)
   - **Store listing**: `play-store-icon-512.png`, a 1024×500 feature graphic, at least 2 phone screenshots

App icon and splash screen are made from the `assets` folder during the build.

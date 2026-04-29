# Vida Cloud — Android Client Brand Status

Fork of [`nextcloud/android`](https://github.com/nextcloud/android) (AGPL-3.0-or-later) rebranded to Vida Cloud.

## What's done

All brand-replaceable strings, IDs, colors, and feature flags live in [`app/src/main/res/values/setup.xml`](app/src/main/res/values/setup.xml) — already wired:

- `app_name` = `Vida Cloud`
- `account_type` = `vidacloud`, `authority` = `com.vidatools.cloud`
- All file/document/image-cache provider authorities renamed to `com.vidatools.cloud.*`
- `webview_login_url` = `https://vidatools.com/cloud/index.php/login/flow` (so login UI is Nextcloud's modern OAuth flow page, served from our domain)
- `enforce_servers` locked to Vida Cloud only — no other servers selectable
- `is_branded_client=true` — suppresses Nextcloud ecosystem-app suggestions
- Brand colors: primary `#6B8F71`, primary_dark `#557159`, accent `#8FB095` (Vida sage palette)
- `splashScreenBold` / `splashScreenNormal` set to "Vida Cloud" / "by Vida Design"
- Source-code link points to `gandfrey/vida-cloud-android` (AGPL requires this)
- License link → `gnu.org/licenses/agpl-3.0.html`
- All flavor `applicationId` values bumped from `com.nextcloud.client` → `com.vidatools.cloud` in [`app/build.gradle.kts`](app/build.gradle.kts) (5 locations: defaultConfig + 4 productFlavors)
- TestRunner reference updated to `com.vidatools.cloud.TestRunner`

## What's still needed before first release

### Signing
1. **Android keystore** — generate once, **back up forever** (losing it = can never push updates to existing installs)
   ```bash
   keytool -genkey -v -keystore vidacloud.keystore -alias vidacloud \
     -keyalg RSA -keysize 4096 -validity 10000
   ```
2. **Google Play Console developer account** — $25 one-time fee — OR distribute APK via direct download from `vidatools.com/cloud/android-app`

### Brand assets
Launcher icons under [`app/src/main/res/`](app/src/main/res/) are still Nextcloud's blue swirl. Replace:

| File | Used for |
|---|---|
| `mipmap-mdpi/ic_launcher.png` (48×48) | Old Android launcher |
| `mipmap-hdpi/ic_launcher.png` (72×72) | |
| `mipmap-xhdpi/ic_launcher.png` (96×96) | |
| `mipmap-xxhdpi/ic_launcher.png` (144×144) | |
| `mipmap-xxxhdpi/ic_launcher.png` (192×192) | |
| `mipmap-anydpi/ic_launcher.xml` | Adaptive icon definition (foreground + background) |
| `drawable/ic_launcher_foreground.xml` | Adaptive icon foreground |
| `drawable/ic_launcher_background.xml` | Adaptive icon background |

**One 1024×1024 master logo** is enough — Android Studio's Image Asset Studio can generate the full mipmap set in one click.

### Play Store listing (if going Play)
- 512×512 store icon
- 1024×500 feature graphic
- 2-8 screenshots per device class (phone + tablet)
- Short description (80 chars), long description (4000 chars)
- Privacy policy URL — `vidatools.com/privacy`

## Build flow

```bash
./gradlew assembleGenericDebug   # debug APK, no signing needed for testing
./gradlew assembleGenericRelease # signed release — requires keystore configured in gradle.properties
./gradlew bundleGplayRelease     # AAB for Play Store upload
```

For first-look testing, the debug APK installs fine via `adb install`.

## License

AGPL-3.0-or-later. We must:
- Keep source publicly available (this fork satisfies that)
- If we deploy any **server-side modifications** to Nextcloud server (e.g., a custom app), publish those too. We're not currently doing that — the server runs stock Nextcloud.

## Upstream sync

```bash
git remote add upstream https://github.com/nextcloud/android.git
git fetch upstream
git merge upstream/master --no-edit
```

Brand strings only live in `setup.xml` + `build.gradle.kts` + the launcher icons. Merge conflicts should be confined to those.

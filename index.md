# Privacy Policy — MY Expense Tracker

**Effective date:** 7 September 2026
**Developer:** Jack Lee (HooiTeikLee)
**Contact:** jacklee.htlee@gmail.com
**App package:** `com.myexpensetracker.my_expense_tracker` (Android)

This policy explains what MY Expense Tracker does — and does not do — with your information. It is written in plain language because the short version is simple: **your data stays on your phone.**

## 1. The short version

- Everything you enter is stored **only on your device**, in the app's private storage.
- There is **no account, no sign-in, no server, no cloud sync**.
- The app has **no analytics, no crash reporting, no advertising, and no third-party tracking SDKs**.
- The app makes **no network requests at all**. (The app contains code for an optional AI advisor and an optional cloud OCR helper, but both are switched off and unreachable in this version — see §6 and §7.) The only network activity related to the app is Google Play delivering updates, which is handled by Google, not by the app.
- The only way data leaves the device is when **you** choose to create a backup file or export it yourself.

## 2. What the app stores on your device

When you use the app, it saves the following in a local database inside the app's private storage area:

| Data | Why |
| --- | --- |
| Profiles (for example "Personal", "Family") | To keep separate sets of finances apart |
| Accounts (name, type, opening balance) | To track balances across bank, cash, card, e-wallet, loan and investment accounts |
| Transactions (date, amount, category, description, account, any custom fields you map during import) | The core of the app |
| Budgets | To compare spending against your limits |
| Saved import mapping templates | So you can reuse a column mapping for the same statement layout |
| Preferences (language, theme, text size, active profile) | To remember how you like the app set up |

The app does **not** ask for or store your name, email address, phone number, bank credentials, card numbers, location, contacts, photos, or any identifier about you or your device.

### Encryption status (please read)

The on-device database is **encrypted at rest** with SQLCipher (AES-256); this is verified on Android, while an iOS build has not yet been produced. The encryption key is generated randomly on your device the first time the app runs and is stored in the Android Keystore (or the iOS Keychain). You never see or type it, it is not derived from anything you know, it is never written to a backup file, and the developer never receives it. If your device loses that key (for example after a factory reset), the encrypted records cannot be recovered by anyone, including the developer; the app will offer to start fresh, after which you can restore a backup file you created.

An optional **app lock** (Profile → Security) lets you require a 6-digit PIN, or your fingerprint/face, to open the app; it is off by default. The PIN is stored only as a salted hash on your device (never the PIN itself, and never in a backup file), the developer never receives it, and there is no way to reset a forgotten PIN other than reinstalling the app and restoring a backup. Biometric matching is performed entirely by your phone's operating system; the app only learns whether it succeeded. Until you turn the lock on, anyone who can unlock your phone can open the app, so we recommend that you:

- keep a screen lock on your phone, and
- avoid entering information you would not want someone with physical access to your unlocked phone to see.

## 3. What the app does not do

- **No data collection.** The developer never receives any data from the app. There is no telemetry of any kind.
- **No data sharing or selling.** Nothing is sent to the developer or to any third party.
- **No advertising.** The app contains no ads and no ad SDKs.
- **No accounts.** You are never asked to register or sign in.
- **No Android system backup.** The app opts out of Android auto-backup and device-to-device transfer, so your financial records are not copied to Google's backup service by the operating system.

## 4. Backups and exports you control

From **Profile → Data → Create backup** you can create an encrypted backup file (`.myexpensebackup`).

- The file is created **only when you tap the button**.
- It is encrypted with a passphrase you choose, using Argon2id key derivation and AES-256-GCM. The app does not store your passphrase and cannot recover it.
- The file is saved **where you choose** using the Android file picker (for example your Downloads folder or a cloud-drive folder you have installed). The app itself never uploads anything.
- Once the file is outside the app, it is your copy to keep, move, share, or delete. If you save it into a third-party cloud folder, that provider's privacy policy applies to the file.

**Restore** reads a backup file you select, decrypts it with your passphrase, and replaces the app's local data after you confirm.

## 5. Files you import

The Import studio lets you choose a CSV/TSV/TXT statement file through the Android file picker. The app reads the file only to build the on-screen review. It does **not** retain or upload the source file; only the rows you explicitly confirm are saved to the local database. A CSV, Excel (`.xlsx`/`.xls`) or text-layer PDF statement is always processed entirely on your device — the one exception is a photographed/screenshotted statement or a scanned (image-only) PDF, which can optionally be read by a third-party cloud OCR service instead of on-device recognition; see the next section for exactly what that involves.

## 6. Optional cloud OCR (Mistral) — not available in this version

The app's code contains an optional cloud OCR path (Import studio "Scan
images" reading a photographed or scanned statement through Mistral, a
third-party cloud OCR provider, instead of on-device recognition), but it is
**compiled out of this release** and has no reachable entry point in the
app's UI — there is no toggle, no consent screen, and no way to turn it on.
Every scan the Import studio performs in this version is read with
on-device, on-your-phone recognition (Google ML Kit); nothing about a
statement image or PDF ever leaves your device, and the app makes no
network request of any kind while scanning. Should this feature ship in a
future version, this policy will be updated first with a new effective
date, describing exactly what would be sent, to whom, and under what
opt-in.

## 7. Optional AI advisor (bring your own provider) — not available in this version

The app's code contains an optional finance advisor chat that could talk to
a large language model (LLM) you configure, but it is **compiled out of
this release** and has no reachable entry point in the app's UI — no
Advisor tile, no consent screen, no way to enable it. The app makes no
network request to any LLM provider in this version. Financial goals you
create are stored and tracked locally on your device regardless, and the
Home tab's "daily check" pace/budget summary is computed entirely on the
device with no advisor or network involvement. Should the advisor ship in a
future version, this policy will be updated first with a new effective
date, describing exactly what would be sent, to whom, and under what
opt-in.

## 8. Permissions

The app requests **no dangerous (runtime) permissions** — no storage, camera, location, contacts, or microphone access.

- File access for import and backup happens through the Android system file picker (Storage Access Framework), which grants the app access only to the single file you pick, only for that operation.
- The Android `INTERNET` permission appears in the app's manifest because the Flutter framework's build tooling includes it by default, and because the optional AI advisor described above uses it, only after you opt in, to reach the provider you configured. No other app code makes network calls.

## 8. Deleting your data

Because there is no copy anywhere else, deleting your data is entirely in your hands:

- **Start fresh:** Profile → Data → Data management → Start fresh permanently erases every profile, account, transaction, budget and template on the device. Starting fresh from the key-loss recovery screen also deletes the unreadable encrypted database file and its key.
- **Uninstall the app:** Android removes the app's private storage, including the database, when you uninstall.
- **Backup files** you created are separate files; delete them wherever you saved them.

No deletion request to the developer is necessary or possible, because the developer holds nothing.

## 9. Children

The app is a personal-finance tool intended for adults and is not directed at children under 18. It does not knowingly collect any information from anyone, including children.

## 10. Changes to this policy

If the app gains features that change how data is handled (for example optional cloud backup to a provider you connect), this policy will be updated before that feature is released, with a new effective date at the top. The current version is always published at:

`https://hooiteiklee.github.io/MYExpenseTracker-privacy/`

(published from the public repository `HooiTeikLee/MYExpenseTracker-privacy`).

## 11. Contact

Questions about this policy: **jacklee.htlee@gmail.com**

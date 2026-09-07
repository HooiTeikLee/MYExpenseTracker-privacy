# Privacy Policy — MY Expense Tracker

**Effective date:** 23 August 2026
**Developer:** Jack Lee (HooiTeikLee)
**Contact:** jacklee.htlee@gmail.com
**App package:** `com.myexpensetracker.my_expense_tracker` (Android)

This policy explains what MY Expense Tracker does — and does not do — with your information. It is written in plain language because the short version is simple: **your data stays on your phone.**

## 1. The short version

- Everything you enter is stored **only on your device**, in the app's private storage.
- There is **no account, no sign-in, no server, no cloud sync**.
- The app has **no analytics, no crash reporting, no advertising, and no third-party tracking SDKs**.
- The app makes **no network requests**, except the optional AI advisor, which only contacts the provider you configure after you opt in. The only other network activity related to the app is Google Play delivering updates, which is handled by Google, not by the app.
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

The current release is still an **early-access preview**. An optional **app lock** (More → Security) lets you require a 6-digit PIN, or your fingerprint/face, to open the app; it is off by default. The PIN is stored only as a salted hash on your device (never the PIN itself, and never in a backup file), the developer never receives it, and there is no way to reset a forgotten PIN other than reinstalling the app and restoring a backup. Biometric matching is performed entirely by your phone's operating system; the app only learns whether it succeeded. Until you turn the lock on, anyone who can unlock your phone can open the app, so we recommend that you:

- keep a screen lock on your phone, and
- avoid entering information you would not want someone with physical access to your unlocked phone to see.

## 3. What the app does not do

- **No data collection.** The developer never receives any data from the app. There is no telemetry of any kind.
- **No data sharing or selling.** Nothing is sent to the developer or to any third party.
- **No advertising.** The app contains no ads and no ad SDKs.
- **No accounts.** You are never asked to register or sign in.
- **No Android system backup.** The app opts out of Android auto-backup and device-to-device transfer, so your financial records are not copied to Google's backup service by the operating system.

## 4. Backups and exports you control

From **More → Backup → Create backup** you can create an encrypted backup file (`.myexpensebackup`).

- The file is created **only when you tap the button**.
- It is encrypted with a passphrase you choose, using Argon2id key derivation and AES-256-GCM. The app does not store your passphrase and cannot recover it.
- The file is saved **where you choose** using the Android file picker (for example your Downloads folder or a cloud-drive folder you have installed). The app itself never uploads anything.
- Once the file is outside the app, it is your copy to keep, move, share, or delete. If you save it into a third-party cloud folder, that provider's privacy policy applies to the file.

**Restore** reads a backup file you select, decrypts it with your passphrase, and replaces the app's local data after you confirm.

## 5. Files you import

The Import studio lets you choose a CSV/TSV/TXT statement file through the Android file picker. The app reads the file only to build the on-screen review. It does **not** retain or upload the source file; only the rows you explicitly confirm are saved to the local database. A CSV, Excel (`.xlsx`/`.xls`) or text-layer PDF statement is always processed entirely on your device — the one exception is a photographed/screenshotted statement or a scanned (image-only) PDF, which can optionally be read by a third-party cloud OCR service instead of on-device recognition; see the next section for exactly what that involves.

## 6. Optional cloud OCR (Mistral)

The Import studio's "Scan images" and "Take photo" flows, and a scanned (image-only) PDF opened through "Open a file", read text with on-device, on-your-phone recognition (Google ML Kit) by default — nothing about that image ever leaves your device. You can optionally turn on cloud OCR instead, which sends that one statement image or PDF to Mistral, a third-party cloud OCR provider, to read it — usually more accurate for a dense or low-quality scan.

- **Off by default, and gated behind an explicit opt-in.** Cloud OCR only activates once you have turned it on, agreed to a dedicated consent screen ("Cloud OCR sends your statement image to Mistral (a third-party cloud service) to read it. Your key is stored only on this device."), and entered your own Mistral API key. Missing any one of those three keeps every scan on-device, and no request is ever sent to Mistral before all three are true.
- **What is sent, and only for the one image/PDF you are scanning:** the photographed or scanned statement image or PDF you selected for that import, base64-encoded inside the request body, plus your API key in the request's authorization header (never as part of the request body/data). Nothing else — no other transaction, account, budget, report or profile data ever reaches this endpoint.
- **Where it goes.** Requests go directly from your device to `api.mistral.ai` over HTTPS. The developer of this app never receives any of it — there is no intermediary server.
- **CSV, Excel and text-layer PDF statements are never affected.** Only a photographed/screenshotted image or an image-only scanned PDF can ever be sent — a source that already has a text layer or a tabular file format is read entirely on-device regardless of this setting.
- **If cloud OCR fails or is unreachable** (network issue, an invalid key, a rate limit, or a timeout), the studio never silently fails or gets stuck: it shows what went wrong and offers a one-tap "Scan on-device instead" that reads the same image locally.
- **Your API key** is stored only in your device's secure keystore (Android Keystore / iOS Keychain) — the same place the database encryption key, the app-lock PIN hash, and the advisor's own API key live. It is never written to the database, never included in a backup file, and never logged.
- **Turning it off.** Cloud OCR settings (reachable from the Import studio) let you disable the toggle or delete your stored key at any time, either of which immediately returns every future scan to on-device recognition.

Because this feature depends on Mistral, its own privacy policy and terms apply to the one image or PDF you send it while cloud OCR is enabled.

## 7. Optional AI advisor (bring your own provider)

The app includes an optional finance advisor chat that can talk to a large
language model (LLM). It is **off by default** and requires an **explicit
opt-in** on a consent screen before anything is enabled.

- **You choose the provider.** From Advisor settings you configure the
  provider (for example Anthropic's Claude, or any server that speaks the
  OpenAI-compatible API — OpenAI, a self-hosted Ollama server, OpenRouter,
  and similar), the model, an optional base URL, and your own API key. The
  app sends requests only to the endpoint you configured.
- **What is sent, and only after you opt in:** a compact summary of your
  data — account balances, goal progress, budgets versus actual spending,
  recent and year-to-date spending, cash flow, income and recurring
  commitments — plus, only if you separately turn on "include recent
  entries", your most recent transactions (capped at 40). If a question
  needs more than the summary, the model itself composes an ad-hoc query
  (a report definition: source, period, filters, grouping) and the app runs
  it on your device, on your existing data only, and sends back just the
  resulting aggregated rows (a feature called `run_report`) — those rows
  can include category, tag and transaction-description labels, since
  those are exactly what a category/tag/description breakdown reports. Each
  `run_report` call returns at most 20 rows, and the advisor can make at
  most 6 such calls while answering a single question. The advisor can also
  propose saving a report it just ran as a custom report on your Reports
  tab, but nothing is saved without your confirmation, and the advisor
  never writes to your data.
- **Where it goes.** By default every request uses HTTPS. Advisor settings
  lets you point the base URL at a plain http:// server instead, but only
  after you explicitly confirm an "unencrypted connection" warning, and
  only for a local or private-network address (for example a self-hosted
  model on your own device or network) — a public http:// address is
  refused outright. The app does not pin or otherwise verify the
  provider's TLS certificate beyond what the operating system already
  does, by design (so any certificate your device already trusts works,
  including a corporate or self-hosted proxy you have configured). Whatever
  address you configure, requests go directly from your device to it.
  **The developer of this app never receives any of it** — there is no
  intermediary server.
- **Your API key** is stored only in your device's secure keystore (Android
  Keystore / iOS Keychain), the same place the database encryption key and
  the app-lock PIN hash live. It is never written to the database, never
  included in a backup file, and never logged.
- **Conversations are not stored.** The chat exists only in memory while the
  advisor screen is open; closing it discards the conversation. Nothing
  about your questions or the advisor's answers is saved to the device or
  anywhere else.
- **Turning it off.** Advisor settings lets you revoke consent at any time,
  which disables the advisor immediately; deleting your API key there also
  removes it from the keystore. Financial goals you created (the targets
  the advisor and the Goals screen track) stay on your device like any
  other data and are covered by the rest of this policy.

Because this feature depends on a third-party provider you choose, that
provider's own privacy policy and terms apply to the data you send them
while the advisor is enabled.

## 7. Permissions

The app requests **no dangerous (runtime) permissions** — no storage, camera, location, contacts, or microphone access.

- File access for import and backup happens through the Android system file picker (Storage Access Framework), which grants the app access only to the single file you pick, only for that operation.
- The Android `INTERNET` permission appears in the app's manifest because the Flutter framework's build tooling includes it by default, and because the optional AI advisor described above uses it, only after you opt in, to reach the provider you configured. No other app code makes network calls.

## 8. Deleting your data

Because there is no copy anywhere else, deleting your data is entirely in your hands:

- **Start fresh:** More → Data management → Start fresh permanently erases every profile, account, transaction, budget and template on the device. Starting fresh from the key-loss recovery screen also deletes the unreadable encrypted database file and its key.
- **Uninstall the app:** Android removes the app's private storage, including the database, when you uninstall.
- **Backup files** you created are separate files; delete them wherever you saved them.

No deletion request to the developer is necessary or possible, because the developer holds nothing.

## 9. Children

The app is a personal-finance tool intended for adults and is not directed at children under 18. It does not knowingly collect any information from anyone, including children.

## 10. Changes to this policy

If the app gains features that change how data is handled (for example optional cloud backup to a provider you connect), this policy will be updated before that feature is released, with a new effective date at the top. The current version is always published at:

`https://hooiteiklee.github.io/MYExpenseTracker/PRIVACY`

and in the source repository at `docs/PRIVACY.md`.

## 11. Contact

Questions about this policy: **jacklee.htlee@gmail.com**

# Family Khata – setup guide (about 20 minutes, one time)

You need: a Google account (for Firebase, free Spark plan) and a GitHub account (you already use GitHub Pages).

## 1. Create the sync backend (Firebase)
1. Go to https://console.firebase.google.com → **Add project** (name it e.g. family-khata). Turn Google Analytics off.
2. **Build → Authentication → Get started → Email/Password → Enable → Save.**
3. **Build → Firestore Database → Create database** → pick a region near India (asia-south1 Mumbai) → Production mode.
4. In Firestore open the **Rules** tab, replace everything with this, then **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /families/{uid}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
  }
}
```
5. **Project settings (gear) → General → Your apps → Web (</>)** → register an app (no hosting). Copy the `firebaseConfig` block shown.

## 2. Host the app (GitHub Pages)
1. Create a new GitHub repository (e.g. `khata`), upload **all files and folders** from this zip (index.html, sw.js, manifest.webmanifest, config.js, icons/, vendor/).
2. Repo **Settings → Pages → Deploy from branch → main / (root)**. After a minute you get `https://<you>.github.io/khata/`.
3. Back in Firebase: **Authentication → Settings → Authorized domains → Add** `<you>.github.io`.

## 3. (Optional, recommended) Pre-fill Firebase settings for every phone
Open `config.js`, replace `null` with your config object (see the example inside), and re-upload. Then phones need no pasting.
If you skip this, paste the firebaseConfig block once on each phone: **More → Sync across phones**.

## 4. Install on each phone
- **Android:** open the link in Chrome → ⋮ menu → **Install app** (or Add to Home screen). It opens full-screen from the home icon.
- **iPhone:** open the link in **Safari** → Share → **Add to Home Screen**.

## 5. Connect the phones
- **First phone:** More → Sync across phones → choose an email and password (this one shared login is the family login) → **Create account**. Then tap **Upload** to send existing local records.
- **Second phone:** paste the setup text (or nothing if you filled config.js), enter the **same** email and password → **Sign in**. Data appears.
- After both phones are connected, in Firebase **Authentication → Settings → User actions** you can disable sign-up so nobody else can create accounts.

## Using offline
The app works without internet; entries sync when the phone is online again.

## Updating the app later
Replace files in the repo. In `sw.js` change `khata-v1` to `khata-v2` so phones refresh. Close and reopen the app twice.

## Backup
More → Backup saves a file to your phone. Do this occasionally.

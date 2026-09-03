# Daily Sittings — meditation tracker

A single-file meditation streak tracker. Two sittings a day; a day keeps your
streak alive as long as you sit at least once. Tracking starts **24 Aug 2026**.

`index.html` is completely self-contained. Open it and it works immediately,
saving your ticks in the browser (`localStorage`). Everything below is the
*optional* setup for **cross-device sync** — so your streak follows you from
laptop to phone — using your own free Firebase project (personal Google
account, nothing tied to work).

If you skip the Firebase steps, the tracker still works fine; it just stays on
one browser.

---

## Host it on GitHub Pages

1. Put this folder in a GitHub repo (a new repo, or a subfolder of one).
2. In the repo: **Settings → Pages**.
3. Under **Build and deployment**, set **Source: Deploy from a branch**, pick
   your branch (e.g. `main`) and folder (`/root` if `index.html` is at the top,
   or `/docs` if you move it there).
4. Save. After a minute your tracker is live at
   `https://<your-username>.github.io/<repo>/`.

Bookmark that URL. Done — if you don't want sync, stop here.

> Note: free GitHub Pages sites are public URLs. Nobody can see your data
> without signing in as you (the Firebase rules below enforce that), but the
> page itself is publicly viewable. That's fine for a personal tracker.

---

## Turn on cross-device sync (Firebase) — about 5 minutes

### 1. Create a Firebase project
- Go to <https://console.firebase.google.com> and sign in with your
  **personal** Google account.
- **Add project** → name it (e.g. `daily-sittings`) → you can disable Google
  Analytics → **Create project**. The free **Spark** plan is all you need.

### 2. Register a web app and copy the config
- In the project, click the **`</>`** (Web) icon to add a web app → give it a
  nickname → **Register app**.
- Firebase shows a `firebaseConfig` object. Copy the values for `apiKey`,
  `authDomain`, `projectId`, and `appId`.
- Open `index.html`, find the **PASTE YOUR FIREBASE WEB CONFIG HERE** block near
  the bottom, and paste your values in. (These are *not* secrets — Firebase web
  config is meant to ship in client code; the rules in step 4 are what protect
  your data.)

### 3. Enable Google sign-in
- **Build → Authentication → Get started**.
- **Sign-in method** tab → enable **Google** → Save.

### 4. Create the database and lock it to you
- **Build → Firestore Database → Create database** → Start in **production
  mode** → pick a location → Enable.
- Open the **Rules** tab, replace everything with the rules below, and
  **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Each signed-in user can read and write only their own document.
    match /meditation/{uid} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
  }
}
```

### 5. Authorize your domains
- **Authentication → Settings → Authorized domains → Add domain**.
- Add `<your-username>.github.io`. `localhost` is already there for local
  testing.

### 6. Use it
- Reload your GitHub Pages URL, click **Sign in to sync**, choose your Google
  account. The footer will read *"synced to your account."*
- Sign in with the same Google account on any other device to see the same
  streak, live. Any ticks you'd already made in that browser migrate up the
  first time you sign in.

---

## How your data is stored

- One Firestore document per user at `meditation/<your-uid>`, holding a `days`
  map: `{ "2026-08-24": { a: true, b: false }, ... }` where `a`/`b` are the two
  sittings.
- No personal info is stored server-side beyond the account id Firebase assigns
  you. Your email is only shown in the header from your live sign-in session; it
  isn't written to the database.
- The Firestore rules mean only you (signed in as you) can read or write your
  document.

## Editing

Everything is in `index.html` — HTML, CSS, and JS in one file. The two `<script>`
blocks are labelled: the first is the tracker itself (works alone); the second
is the optional Firebase sync layer.

# Daily Sittings

A meditation streak tracker. Two sittings a day. A day keeps the streak if you
sit at least once. Practice starts **24 Aug 2026**.

Open `index.html` in a browser. Tick a sitting when you finish it. The page
stores ticks on this device.

## What you see

- Current streak, best streak, total sittings, sittings this month, and days
  practiced
- A month calendar. Each day has two dots, one per sitting
- Light days: one sitting. Darker days: both sittings

You can move between months. Days before the start date, and future days, are
locked.

## Cross-device sync (optional)

The page can sync ticks across devices with Firebase and Google sign-in. Until
you add a Firebase config in `index.html`, ticks stay in this browser only.

1. Create a Firebase project at <https://console.firebase.google.com> (the free
   Spark plan is enough).
2. Add a web app and copy `apiKey`, `authDomain`, `projectId`, and `appId` into
   the `firebaseConfig` block near the bottom of `index.html`.
3. Enable **Google** sign-in under Authentication.
4. Create a Firestore database in production mode. Use these rules:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /meditation/{uid} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
  }
}
```

5. Under Authentication → Settings → Authorized domains, add the domain that
   hosts the page (for GitHub Pages: `<your-username>.github.io`). `localhost`
   is already allowed.

Reload the page, click **Sign in to sync**, and use the same Google account on
each device. Each signed-in user can read and write only their own document.

## Hosting

This folder is a static site. GitHub Pages can serve `index.html` from the
repo root. The site URL is public; sitting data is not in the repo.

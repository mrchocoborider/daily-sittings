# Daily Sittings

A personal meditation streak tracker. Two sittings a day. A day keeps the streak
if you sit at least once. Practice starts **24 Aug 2026**.

Open `index.html` in a browser. Tick a sitting when you finish it. The page
stores ticks on this device.

## What you see

- Current streak, best streak, total sittings, sittings this month, and days
  practiced
- A month calendar. Each day has two dots, one per sitting
- Light days: one sitting. Darker days: both sittings

You can move between months. Days before the start date, and future days, are
locked.

## Sync (optional)

The page can sync ticks across devices with Firebase and Google sign-in. Until
you add your own Firebase config in `index.html`, ticks stay in this browser
only. Setup notes are in `SETUP.md` on this machine (not in git).

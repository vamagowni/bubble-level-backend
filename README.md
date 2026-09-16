# Bubble Level Backend

Firestore backend for the [Bubble Level iOS app](https://github.com/vamagowni/bubble-level-ios) — a CS4261 assignment.

## What this is

A Cloud Firestore database (project ID: `bubble-level-backend`) that the iOS app writes to over Firestore's REST API whenever the user taps "Save Reading." Each saved document captures the device's tilt at that moment.

## How the app talks to it

The app POSTs directly to the Firestore REST endpoint (no SDK, no auth) from `FirestoreService.swift`:

```
POST https://firestore.googleapis.com/v1/projects/bubble-level-backend/databases/(default)/documents/readings
```

with a JSON body like:

```json
{
  "fields": {
    "pitch": { "doubleValue": 12.5 },
    "roll": { "doubleValue": -3.2 },
    "source": { "stringValue": "ios-app" }
  }
}
```

## Files

- `firestore.rules` — security rules (currently in open test mode, expires 2026-10-15)
- `firestore.indexes.json` — Firestore index config
- `firebase.json` — Firebase CLI project config

## Setup

1. Install the Firebase CLI: `npm install -g firebase-tools`
2. `firebase login`
3. `firebase deploy --only firestore:rules,firestore:indexes` (from this directory) to push rule changes to the live project

## Known limitation

Security rules currently allow open read/write with no authentication — fine for a class demo, not production-safe. A real deployment would need Firebase Auth + rules scoped to `request.auth.uid`.

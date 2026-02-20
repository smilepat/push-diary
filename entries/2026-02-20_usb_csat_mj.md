# Dev Diary - usb_csat_mj_generator

**Date**: 2026-02-20
**Project**: usb_csat_mj_generator (KSAT Item Generator)

## Tasks Completed

### 1. Firebase Firestore Integration for Vercel Data Persistence
- Vercel serverless environment loses SQLite data on every redeployment
- Integrated Firebase Firestore as persistent database for production
- Local development continues using SQLite (no change needed)

### 2. Firebase Project Setup
- Created Firebase project (`usb-mj-csat-generator`) on Firebase Console
- Activated Firestore database
- Configured security rules (server-only access via service account)
- Generated service account key and set as Vercel environment variable (`FIREBASE_SERVICE_ACCOUNT`)

### 3. Vercel Production Deployment
- Deployed to Vercel with Firebase integration
- Verified health check: `"database": "firestore", "firebase": true`
- App URL: `https://web-app-pi-ruby.vercel.app`

### 4. Project Config Cleanup
- Removed unused `.clasp.json` (Google Apps Script config)
- Consolidated `.claude/settings.json` into `settings.local.json`
- Updated `.gitignore` rules
- Pushed all changes to GitHub

## Key Files Changed

| File | Action | Description |
|------|--------|-------------|
| `web-app/server/db/firestore.js` | Created | Firestore client with SQLite-compatible interface |
| `web-app/server/db/database.js` | Modified | Added Firebase/SQLite dual-mode support |
| `web-app/server/index.js` | Modified | Health check shows DB type, Vercel DB init |
| `web-app/package.json` | Modified | Added `firebase-admin` dependency |
| `web-app/FIREBASE_SETUP.md` | Created | Firebase setup guide with security checklist |
| `pushgithub.md` | Created | Dev diary push workflow instructions |

## Technical Decisions

- **Dual DB architecture**: SQLite for local dev, Firestore for Vercel production
- **Auto-fallback**: If Firebase init fails, automatically falls back to SQLite
- **SQL-to-Firestore adapter**: `FirestoreStatement` class parses SQL queries and translates them to Firestore operations, minimizing changes to existing code
- **Seed data**: Firestore auto-seeds from `seed_prompts.json` on first initialization

## Architecture

```
Local Dev:    App -> SQLite (file-based, /data/csat.db)
Vercel Prod:  App -> Firebase Firestore (cloud, persistent)
```

Detection logic: `USE_FIREBASE = process.env.FIREBASE_SERVICE_ACCOUNT && process.env.VERCEL`

## Security Notes

- Service account private key was exposed in conversation - user advised to rotate key
- Firestore rules set to deny all client access (`allow read, write: if false`)
- Data accessible only via server-side Admin SDK

## Pending Items

- Rotate Firebase service account key (security recommendation)
- Verify end-to-end data persistence after redeployment
- Test all CRUD operations through Firestore in production

# SessionBox Dashboard

Web-based session manager with Google Sign-in and cloud sync.  
Hosted on GitHub Pages — **no server required**.

## 🚀 Deploy to GitHub Pages

1. Create a new GitHub repo (e.g. `sessionbox-dashboard`)
2. Copy these files into it:
   - `index.html`
   - `404.html`
   - `README.md`
3. Go to **Settings → Pages**
4. Set source to **Deploy from branch → main → / (root)**
5. Your dashboard is live at `https://yourusername.github.io/sessionbox-dashboard`

## 🔥 Firebase Setup (one-time)

Your Firebase project needs two things enabled:

### 1. Enable Google Auth
- Firebase Console → **Authentication → Sign-in method**
- Enable **Google**
- Add your GitHub Pages URL to **Authorized domains**:  
  `yourusername.github.io`

### 2. Create Firestore Database
- Firebase Console → **Firestore Database → Create database**
- Start in **production mode**
- Add these security rules:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    match /sb_users_{userId}_{path=**}/{doc} {
      allow read, write: if request.auth != null
        && request.auth.uid == userId;
    }
  }
}
```

## 🔗 Connecting the Extension

The extension uses the same Firebase project for sync.  
When a user signs into the dashboard with Google, their sessions are stored in Firestore  
and the extension reads from the same database.

**Extension data path:** `sb_users_{uid}_private_sessions / {sessionId}`

## Features

- ✅ Google Sign-in (free, unlimited users)
- ✅ Cloud sync via Firestore (free tier: 50k reads/day, 20k writes/day)
- ✅ Local-only mode for guest users (localStorage)
- ✅ Create / edit / delete sessions
- ✅ Export sessions as JSON
- ✅ Import sessions from JSON file
- ✅ Auto-sync toggle
- ✅ Works offline (reads from localStorage cache)

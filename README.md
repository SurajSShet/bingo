# 🎉 Multiplayer Bingo PWA

> Real-time multiplayer Bingo for up to 10 players — powered by Firebase Firestore, installable as a PWA, and hostable on GitHub Pages for free.

![Bingo](icons/icon-192.png)

---

## ✨ Features

- 🎲 **5×5 Bingo grid** with shuffled numbers 1–25 per player
- 👥 **Up to 10 players** per room
- 🔄 **Turn-based number calling** — only the active player can call
- 🏆 **Win detection** — rows, columns, both diagonals (5 lines = BINGO!)
- 💬 **Real-time chat** in every room
- 📱 **PWA** — installable on Android, iOS, and desktop
- 🌐 **GitHub Pages** friendly — zero backend required

---

## 🔧 Firebase Setup (Required)

### 1. Create a Firebase Project

1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. Click **Add project** → name it (e.g., `bingo-pwa`) → Create
3. Skip Google Analytics if you want

### 2. Enable Services

#### Firestore Database
- Go to **Build → Firestore Database**
- Click **Create database**
- Select **Start in test mode** (you'll update rules later)
- Choose a region → Done

#### Anonymous Authentication
- Go to **Build → Authentication**
- Click **Get started**
- Under **Sign-in method**, enable **Anonymous** → Save

### 3. Get Your Config

1. Go to **Project Settings** (gear icon)
2. Under **Your apps**, click **</> Web**
3. Register app (name it anything)
4. Copy the `firebaseConfig` object shown

### 4. Paste Config into the HTML Files

Open both `index.html` and `game.html` and replace the placeholder block:

```javascript
// ── PASTE YOUR FIREBASE CONFIG HERE ──────────────────────────────────────
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

### 5. Firestore Security Rules

In **Firestore → Rules**, paste these rules and click **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /rooms/{roomId} {
      allow read, write: if request.auth != null;
      match /chat/{msgId} {
        allow read, write: if request.auth != null;
      }
    }
  }
}
```

---

## 🗂️ Project Structure

```
bingo/
├── index.html          ← Landing page (create/join room)
├── game.html           ← Game room (grid, chat, players)
├── css/
│   └── style.css       ← Ultra-modern dark UI
├── icons/
│   ├── icon-192.png    ← PWA icon
│   └── icon-512.png    ← PWA icon
├── manifest.json       ← PWA manifest
├── service-worker.js   ← Offline caching
└── README.md
```

---

## 🚀 Deploy to GitHub Pages

### Option A — Push and Enable Pages
```bash
git add .
git commit -m "🎉 Initial Bingo PWA"
git push origin main
```
Then in your repo: **Settings → Pages → Branch: main / root → Save**

Your game will be live at:
```
https://<your-username>.github.io/bingo/
```

### Option B — Local Development
Use any static file server, e.g.:
```bash
# Python
python -m http.server 8080

# Node.js (npx)
npx serve .
```
> **Important:** Service workers require `localhost` or HTTPS. Use a proper server, not `file://`.

---

## 🎮 How to Play

1. **Open the site** and enter your username
2. **Create a room** → share the 6-letter Room ID with friends
3. Friends **join** using the Room ID
4. The **host** clicks **Start Game**
5. Players **take turns calling numbers** — click a number on your board or type it in
6. Numbers are highlighted across all players' boards
7. First player to complete **5 lines (rows/columns/diagonals)** wins!
8. Click **Play Again** to reset and replay in the same room

---

## 📱 Install as PWA

| Platform | Steps |
|----------|-------|
| Android (Chrome) | Menu → **Add to Home screen** |
| iOS (Safari) | Share → **Add to Home Screen** |
| Desktop (Chrome/Edge) | Install icon (⊕) in address bar |

---

## 🔐 Notes & Limits

- Firestore free tier: **50k reads / 20k writes / 20k deletes per day** — plenty for personal use
- Rooms are not automatically deleted; you can add a Cloud Function later to clean up old rooms
- Max **10 players** per room (enforced client-side)
- Each player gets a **unique random board** per session

---

## 🧩 Optional Enhancements

- [ ] Add sound effects (Web Audio API)
- [ ] Confetti already included in winner modal ✅
- [ ] Add dark/light mode toggle
- [ ] Local leaderboard with `localStorage`
- [ ] Share room link button (already shows Room ID in header)
- [ ] Room expiry using Firestore TTL or Cloud Functions

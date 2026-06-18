# EDcreator Firebase Multi-User Setup

This version turns EDcreator into a multi-user cloud dashboard.

## What changed

- Login / Create Account screen using Firebase Authentication.
- Every account gets a separate Realtime Database path:

```txt
users/{uid}/dashboard
```

- New users start with completely empty dashboard data.
- User data syncs across phone/PC when logged into the same account.
- The old browser-only localStorage system and PIN lock are removed from the active flow.

## 1. Firebase project

Your Firebase config is already pasted into `index.html` for project `edcreator-800bf`.

Important: Realtime Database URL is currently set as:

```txt
https://edcreator-800bf-default-rtdb.firebaseio.com
```

If Firebase Console shows a different URL after you create Realtime Database, open `index.html`, search for `databaseURL`, and replace only that URL.

## 2. Enable Authentication

Firebase Console → Authentication → Sign-in method → enable **Email/Password**.

## 3. Create Realtime Database

Firebase Console → Realtime Database → Create Database.

Choose a region close to your users.

## 4. Add secure database rules

Realtime Database → Rules → paste this:

```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "auth != null && auth.uid === $uid",
        ".write": "auth != null && auth.uid === $uid"
      }
    }
  }
}
```

This makes every user private. A user can only read/write their own `users/{uid}` data.

## 5. GitHub Pages deploy

1. Create a GitHub repository.
2. Upload `index.html`.
3. Go to repository Settings → Pages.
4. Source: Deploy from branch.
5. Branch: `main`, folder: `/root`.
6. Save.
7. Open your GitHub Pages link.

## 6. Test

1. Create account A.
2. Add a transaction/note.
3. Logout.
4. Create account B.
5. Account B should be completely empty.
6. Login account A again.
7. Account A data should come back.

## Important

Your Firebase config is okay to be inside frontend code. The real protection comes from Firebase Authentication + Realtime Database security rules.

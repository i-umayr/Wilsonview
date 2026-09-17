# Groceries: shared list for Umair, Mubashir, Zain & Noman

One file (`index.html`), hosted on GitHub Pages, with Firebase Firestore storing the list so everyone sees the same thing live.

## 1. Create the Firebase database (about 5 minutes)

1. Go to https://console.firebase.google.com and click **Create a project**. Any name works (e.g. `roommate-groceries`). You can turn off Google Analytics.
2. In the left menu open **Build → Firestore Database → Create database**.
   - Pick a location near you.
   - Choose **Start in production mode** (we'll set proper rules next; "test mode" stops working after 30 days).
3. Open the **Rules** tab, replace everything with the rules below, and click **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /items/{itemId} {
      allow read, delete: if true;
      allow create, update: if request.resource.data.name is string
        && request.resource.data.name.size() > 0
        && request.resource.data.name.size() <= 80
        && request.resource.data.addedBy in ['Umair', 'Mubashir', 'Zain', 'Noman'];
    }
  }
}
```

4. Click the gear icon → **Project settings** → scroll to **Your apps** → click the web icon **`</>`**. Give it a nickname, skip Hosting, and click **Register app**.
5. Copy the `firebaseConfig = { ... }` values it shows you.

## 2. Paste the config

Open `index.html`, find `PASTE YOUR FIREBASE CONFIG HERE`, and replace the placeholder values with yours.

(Firebase web config keys are designed to be public, so it's fine for them to be visible on GitHub. The rules above are what protect the database.)

## 3. Put it on GitHub Pages

1. Create a new **public** repository on GitHub (e.g. `groceries`).
2. Upload `index.html` (and this README if you like) with **Add file → Upload files**, then commit.
3. Go to **Settings → Pages**. Under **Build and deployment**, set Source to **Deploy from a branch**, branch **main**, folder **/ (root)**, and save.
4. After a minute or two your link appears at the top of that page, like `https://yourusername.github.io/groceries/`. Share it in your group chat.

## 4. Add it to your home screen

- **iPhone (Safari):** Share button → **Add to Home Screen**.
- **Android (Chrome):** ⋮ menu → **Add to Home screen**.

## How it works

- First visit on each phone: tap your name. Tap your magnet (top right) to switch.
- Type an item plus an optional amount or note, then tap **+**. Newest items go on top.
- Tap the circle to mark something bought. It moves to **Recently bought** with your name. Undo appears for a few seconds.
- **Need again** puts a bought item back on the list.
- Adding something already on the list shows a warning. Adding something that's in Recently bought just moves it back up.
- Bought items clear themselves after 7 days.
- Works with bad signal in the store: changes sync once you're back online.

## Changing names later

Edit the `ROOMMATES` list near the top of the script in `index.html`, and update the names in the Firestore rules to match.
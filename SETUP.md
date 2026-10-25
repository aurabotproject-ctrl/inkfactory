# Ink Works multiplayer setup

Solo play works straight away. Multiplayer (Hide & Seek) needs a Firebase Realtime Database.

## 1. Firebase project
1. Go to console.firebase.google.com and create a project (or use an existing one).
2. **Build > Authentication > Get started > Sign-in method**: turn on **Anonymous**.
3. **Build > Realtime Database > Create database** (choose the Singapore / asia-southeast1 location for NZ).
4. **Realtime Database > Rules**: paste the contents of `database.rules.json` and press **Publish**.
   - If the project already has rules (for example Chickens vs Turkeys), do not replace them. Copy only the `"inkhide": { ... }` block and paste it inside the existing `"rules": { ... }`, next to the other top-level entries.
5. **Project settings > General > Your apps**: add a **Web app** (</> icon) and copy its config.

## 2. Paste the config
Open `firebase-config.js` and replace the `PASTE_...` values with your config (apiKey, authDomain, databaseURL, projectId, appId).

## 3. Put it online
Upload the whole `inkfactory` folder (index.html, firebase-config.js) to GitHub and turn on GitHub Pages (or deploy with Netlify).
In Firebase **Authentication > Settings > Authorised domains**, add your site's domain (for example `yourname.github.io`).

## How a game works
- The host presses **Multiplayer > Host a game**, picks the scene, seeker countdown and drawing time, and shares the 4-letter code or the link.
- Up to 10 players join with the code. The host presses **Start game**.
- Every round one player seeks (everyone gets a turn, in join order). Everyone else draws a hider; it must be at least as big as the owl.
- Hiders get 20 seconds to choose a room, then seeking starts.
- Hiders must move to a dotted (neighbouring) room at least every 20 seconds or their drawing shakes for a second. They can't go back to the room they just left.
- Every room change takes 5 seconds off the seeker's countdown and earns that hider 1 point.
- Every seeker tap that misses a hider also takes 5 seconds off the countdown.
- The seeker only sees the hiders' names, not their drawings.
- The seeker's clock stops when every hider is found, or the round ends when the countdown reaches 0.
- Two winners at the end: the **seeker winner** (most hiders found, then most time left) and the **hider winner** (most room-change points, ties broken by rounds survived).

Multiplayer games are stored under `inkhide/<code>` in the database. The host's game is deleted when the host presses Leave.

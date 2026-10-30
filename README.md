# Yardillion

A daily NFL and college football rarity trivia game. Seven prompts, 25 seconds each. The rarer your right answer, the more yards you gain, and only one answer per board is a 100-yard touchdown.

The whole game is one file, `index.html`. Without any setup it runs in guest mode (history saved in each player's browser). Follow the steps below to add sign-in, saved history on any device, and a shared daily leaderboard with real percentiles.

## 1. Put it on GitHub Pages (about 5 minutes)

1. Create a new public repository on GitHub, for example `yardillion`.
2. Upload `index.html` (and this README if you like) to the repository's main branch.
3. Go to **Settings → Pages**. Under **Build and deployment**, choose **Deploy from a branch**, pick `main` and `/ (root)`, and save.
4. After a minute your game is live at `https://YOUR-USERNAME.github.io/yardillion/`.

At this point people can play, but there's no login yet.

## 2. Add sign-in and leaderboards with Firebase (about 10 minutes, free)

Firebase's free Spark plan covers sign-in and the database for a game like this. No credit card is needed.

1. Go to https://console.firebase.google.com and click **Create a project**. Analytics is optional; you can turn it off.
2. **Turn on sign-in methods.** Open **Build → Authentication → Get started**, then on the **Sign-in method** tab enable **Google** and **Email/Password**.
3. **Allow your site's address.** Still in Authentication, open **Settings → Authorized domains** and add `YOUR-USERNAME.github.io`.
4. **Create the database.** Open **Build → Firestore Database → Create database**, choose a location near your players, and start in **production mode**.
5. **Add the security rules.** In Firestore, open the **Rules** tab, replace everything with the contents of `firestore.rules`, and click **Publish**. These rules let each player read and write only their own history, and post exactly one daily score per day.
6. **Get your web config.** Open **Project settings** (the gear icon) → **Your apps** → the **Web** (`</>`) button. Register an app (any nickname, no hosting needed). Firebase shows a `firebaseConfig` object.
7. **Paste it into the game.** In `index.html`, find this line:

   ```js
   const FIREBASE_CONFIG=null;
   ```

   and replace `null` with your config, for example:

   ```js
   const FIREBASE_CONFIG={apiKey:"AIza...",authDomain:"yardillion-123.firebaseapp.com",projectId:"yardillion-123",appId:"1:123:web:abc"};
   ```

8. Commit the change. GitHub Pages republishes in about a minute.

The Firebase web config is meant to be public; it identifies your project but doesn't grant access. The security rules are what protect your players' data.

## What players get

- **A sign-in prompt** when they open the site: Continue with Google, or email and password (with a display name and password reset). They can also play as a guest. Guests' drives are saved in their browser and move to their account the first time they sign in.
- **My history:** every drive they've played, filterable by mode, with averages, best score, touchdown count and a chart of recent drives.
- **Today's leaderboard** after each daily drive: the top 10 and their own rank.
- **Real percentiles:** once 10 players have finished the day's drive, the daily percentile is calculated from everyone's actual scores instead of the built-in estimate.

## Good to know

- Everyone gets the same seven daily prompts. The day turns over at each player's local midnight.
- One score per player per day goes on the leaderboard: the first daily drive they finish while signed in.
- Unlimited and Real Data Dive drives are saved to history but don't count toward the leaderboard.
- The free plan allows 50,000 database reads and 20,000 writes per day, which is plenty for a group of friends or a class. Each results screen reads that day's scores once.
- To reset a day's leaderboard or remove a player, delete their document in the Firestore console under `daily/{day}/scores`.
- Real Data boards come from nflverse's open NFL draft records (1980–2026) and game-day rosters (1995–2025), sourced from Pro Football Reference and NFL rosters.

# Grand National 2026 Sweepstake - Setup Guide

## Quick Start

1. **Set up Firebase** (free tier is more than enough)
2. **Configure the app** with your Firebase credentials
3. **Add your list of people and horses** (dummy data included to test with)
4. **Host on GitHub Pages**
5. **Share the link** and let people draw their horses!

---

## Step 1: Firebase Setup

### Create a Firebase Project
1. Go to [Firebase Console](https://console.firebase.google.com)
2. Click **"Add project"**
3. Name it something like `grand-national-sweepstake`
4. Disable Google Analytics (not needed) and click **Create**

### Enable Realtime Database
1. In your project, go to **Build → Realtime Database**
2. Click **Create Database**
3. Choose a location closest to you (e.g., `europe-west1` for Ireland/UK)
4. Start in **Test Mode** (you can lock it down later)

### Get Web App Config
1. Go to **Project Settings** (gear icon in top left)
2. Under "Your apps", click the **Web** icon (`</>`)
3. Register your app (name doesn't matter, e.g. "sweepstake")
4. Copy the config values and paste them into the `firebaseConfig` object in `index.html`

```javascript
const firebaseConfig = {
    apiKey: "AIzaSy...",
    authDomain: "your-project.firebaseapp.com",
    databaseURL: "https://your-project-default-rtdb.europe-west1.firebasedatabase.app",
    projectId: "your-project",
    storageBucket: "your-project.appspot.com",
    messagingSenderId: "000000000000",
    appId: "1:000000000000:web:xxxxxxxxxxxx"
};
```

### (Optional) Secure the Database
After setup, you can update the database rules. For a small group of friends, test mode is fine:
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

---

## Step 2: GitHub Pages Hosting

### Create a New Repository
1. Go to [GitHub](https://github.com) and create a new repository (e.g., `grand-national-sweepstake`)
2. Upload `index.html` (and this `SETUP_GUIDE.md` if you like)
3. Go to **Settings → Pages**
4. Under "Source", select **Deploy from a branch**
5. Choose **main** branch and **/ (root)** folder
6. Click **Save**
7. Your site will be live at `https://yourusername.github.io/grand-national-sweepstake/`

---

## Step 3: Admin Access

The admin panel is accessed by adding `?admin=grandnational2026` to the URL:

```
https://yourusername.github.io/grand-national-sweepstake/?admin=grandnational2026
```

This reveals the **⚙️ Admin** tab in the navigation bar.

### Changing the Admin Password
To change the admin password, edit the `adminPassword` value in the `appState.settings` object in `index.html`:

```javascript
settings: {
    drawOpen: true,
    predictionsOpen: true,
    adminPassword: 'your-new-password'
}
```

---

## Step 4: Add Your People

1. Open the app with the admin URL (`?admin=grandnational2026`)
2. Go to **Admin → Manage People**
3. Click **Load Current** to see the dummy names
4. Replace with your actual list of 34 people — **one name per line**
5. Click **Save People**

### Example:
```
John
Sarah
Mike
Emma
... (34 names total)
```

---

## Step 5: Update the Horses

The app comes pre-loaded with 34 dummy horses. When the final Grand National field is confirmed (usually a few days before the race), you'll need to update them.

### Option A: Edit via Admin Panel
1. Go to **Admin → Manage Horses**
2. Click **Load Current Data** to see the current JSON
3. Edit the JSON with the real runners and riders
4. Click **Save Horses**

### Option B: Edit Directly in Code
Find the `DEFAULT_HORSES` array in `index.html` and replace with the real data.

### Horse Data Format
Each horse needs these fields:
```json
{
    "number": 1,
    "name": "Horse Name",
    "jockey": "Jockey Name",
    "trainer": "Trainer Name",
    "age": 9,
    "weight": "11-10",
    "odds": "8/1",
    "oddsDecimal": 9.0,
    "color": "#1a5276",
    "colorSecondary": "#f1c40f",
    "aiWriteup": "A paragraph about the horse's chances...",
    "chance": 72
}
```

| Field | Description |
|-------|-------------|
| `number` | Race cloth number (1-34) |
| `name` | Horse's name |
| `jockey` | Jockey's name |
| `trainer` | Trainer's name |
| `age` | Horse's age in years |
| `weight` | Carrying weight (e.g., "11-10" = 11 stone 10 lbs) |
| `odds` | Display odds (e.g., "8/1") |
| `oddsDecimal` | Decimal odds for filtering (e.g., 9.0) |
| `color` | Primary silk colour (hex code) — used for the card header |
| `colorSecondary` | Secondary silk colour (hex code) — used for gradient |
| `aiWriteup` | AI-generated analysis/form guide for the horse |
| `chance` | Win probability percentage (0-100) — used for the chance meter |

### Getting the Real Data
When the final declarations are made:
- The full list of runners and riders is published at [Racing Post](https://www.racingpost.com) and [Sporting Life](https://www.sportinglife.com)
- Odds can be found at any bookmaker (Betfair, Paddy Power, etc.)
- Silk colours can be found on the Racing Post race card

---

## Step 6: Running the Draw

### Before the Draw
1. Make sure all 34 people and 34 horses are loaded
2. Go to **Admin → Draw Control**
3. Click **"Pre-Generate Draw"** — this secretly assigns every person a random horse
4. Tick **"Draw is open"**
5. Share the link with everyone

### How the Pre-Generated Draw Works
The draw is completely fair — it's randomized behind the scenes by the admin:
- The admin pre-generates all horse assignments with one click
- Nobody (including the admin) can influence who gets which horse
- When each person visits the site and spins the wheel, it dramatically reveals their pre-assigned horse
- The wheel animation is purely theatrical — the outcome is already decided
- This prevents anyone from drawing on behalf of another person, since the assignment is already made
- All draws are shown openly on the Sweepstake Board once generated

### During the Draw
1. Each person visits the site
2. They select their name from the dropdown
3. They click **"SPIN THE WHEEL!"**
4. The wheel spins with a dramatic animation and lands on their pre-assigned horse
5. Confetti explodes and they see their horse with a "Lucky or Unlucky?" rating
6. Once revealed, they can revisit their horse anytime

### 🎥 Draw Reveal Show (for recording or presenting live)
Want to create a video of the draw reveal? Or present it live at the pub?
1. Go to **Admin → Draw Control**
2. Click **"🎥 Launch Draw Reveal Show"**
3. A full-screen cinematic presentation plays through all 34 draws:
   - Each person's name appears first
   - A drumroll builds suspense
   - Their horse is revealed with silk colours, odds, and details
   - After all 34, a full summary is shown
4. **Screen record** this for a video to share in the group chat!

### Resetting
If something goes wrong, use **"Reset All Draws"** in Admin to start over.

---

## Step 7: Predictions (Optional Fun Feature)

Before the race, people can submit their predicted Top 3 finishers:
1. Make sure **"Predictions are open"** is ticked in Admin
2. People go to the **Predictions** tab
3. They select their name and pick 1st, 2nd, 3rd
4. All predictions are displayed on a board
5. **Charts update automatically** showing:
   - 🔥 **Most Popular Picks** — a bar chart of which horses are picked most (any position)
   - 🥇 **Picked to Win** — a doughnut chart of who people think will win
6. Close predictions before the race starts by unticking in Admin

---

## Step 8: Race Day — Entering Results

After the race finishes:
1. Go to **Admin → Enter Results**
2. Select the horse that finished 1st, 2nd, 3rd, etc.
3. Click **Save Results**
4. The **Race Day** tab will show the results table with who drew each placed horse

---

## How It Works (No Firebase)

The app works without Firebase too! If you don't configure Firebase credentials:
- Data is stored in **localStorage** in the browser
- Works great for testing and personal use
- The sync status will show **"🟡 Local Mode"**
- Data won't sync across devices — each browser has its own copy

This is perfect for testing before you set up Firebase.

---

## Data Backup & Recovery

### Export
1. Go to **Admin → Data**
2. Click **"Export All Data (JSON)"**
3. A JSON file is downloaded with all horses, people, draws, predictions, and results

### Import
1. Go to **Admin → Data**
2. Click **"Import Data"**
3. Select a previously exported JSON file
4. All data is restored

### Tip
Export a backup before the race in case anything goes wrong!

---

## Customisation Tips

### Changing the Race Date
Update the countdown target date in the `updateCountdown()` function:
```javascript
const raceDate = new Date('2026-04-11T16:00:00+01:00'); // BST
```

### Changing the Theme
The colour scheme is defined using CSS variables at the top of the `<style>` block:
```css
:root {
    --primary: #1a1a2e;
    --secondary: #16213e;
    --accent: #e94560;
    --gold: #ffd700;
    --green-turf: #2d8a4e;
    --green-dark: #1a5c2a;
}
```

### Prize Structure
Edit the welcome text on the Home page to reflect your prize structure (e.g., "Entry fee €5, winner takes 60%, 2nd gets 25%, 3rd gets 15%").

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| **Wheel doesn't spin** | Make sure you've selected a name from the dropdown first |
| **"Draw hasn't been generated"** | The admin needs to pre-generate the draw in Admin → Draw Control |
| **"Draw is closed"** | The admin needs to tick "Draw is open" in Admin → Draw Control |
| **Data not syncing** | Check Firebase config is correct and database rules allow read/write |
| **Admin tab not showing** | Add `?admin=grandnational2026` to the URL |
| **Can't submit prediction** | Predictions may be closed — check Admin → Draw Control |
| **Charts not showing** | Charts appear after at least one prediction is submitted |
| **Reveal show won't launch** | Generate a draw first in Admin → Draw Control |

---

## Timeline Checklist

| When | What to do |
|------|-----------|
| **Now** | Set up Firebase, deploy to GitHub Pages, test with dummy data |
| **When you have participants** | Add real names in Admin → Manage People |
| **When the field is declared** (~3 days before race) | Update horses with real runners, jockeys, odds |
| **Draw day** | Pre-generate the draw, record the Reveal Show, share the link |
| **Before the race** | Open predictions, let people submit their Top 3. Check the charts! |
| **Race day** | Close predictions, watch the race! |
| **After the race** | Enter results in Admin, celebrate the winner! |

---

Enjoy the Grand National! 🏇🎉

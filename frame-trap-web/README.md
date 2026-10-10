# Frame Trap

A six-button arcade fighting game that plays in the browser: https://onixking00.github.io/frame-trap/

This repository is the whole site, hosted free on GitHub Pages. `index.html` is the game on computers, `m.html` on
phones and tablets (phones are sent there automatically). Arcade, Versus, VS CPU and Training work on their own; the
**ONLINE** mode (accounts, profile pictures, rooms, find-a-match, leaderboard) uses the Firebase project described
below.

## 1. Firebase

The game is already pointed at your Firebase project `gbas-gbos-12ecb` (the one Sabi Deck uses). Players can use the
same account in both games. Everything Frame Trap stores sits in its own folder, `ft`, in the Realtime Database, so
it never touches Sabi Deck's data.

Two things must be true in that project:

1. **Build > Authentication > Sign-in method:** Email/Password is enabled.
2. **Build > Realtime Database > Rules:** the rules include the `"ft": { ... }` block from `ft-rules-block.txt`,
   added next to the rules already there. **Do not replace the existing rules with it**: that would lock Sabi Deck
   out.

The Firebase settings are in `fb-config.js` and are also built into the two pages, so the game works even if that
file goes missing. They are not secret: they only say which project the game talks to; the rules protect the data.

## 2. Hosting on GitHub Pages

- The site is switched on under **Settings > Pages**: Source **Deploy from a branch**, branch **main**, folder
  **/ (root)**. The repository must be **Public** for this to work on a free account.
- To update the game, upload the new files over the old ones (**Add file > Upload files**) and commit. The live site
  follows within a minute or two. There is no limit on how often you can do this.
- In Firebase, **Authentication > Settings > Authorized domains** must include `onixking00.github.io`.
- The link preview (WhatsApp and others) is built for the address above. If the repository is renamed or moved, the
  pages need rebuilding with the new address.

## 3. Try it

Open the site on two devices (or two browser windows). On each: **ONLINE**, create an account, choose a fighter
name and picture. On one press **Create a room** and give the four-letter code (or the invite link) to the other,
who types it and presses **Join room**. Or press **Find a match** on both.

## How online play works, in short

- Each player signs in with email and password. The email is never shown to anyone; the fighter name and picture are.
- A profile picture is one of the twelve fighters or an uploaded photo, which is cropped and shrunk to a tiny square
  before it is saved.
- The two devices connect directly to each other for the fight. On networks that block direct links (some mobile
  data), the game falls back to passing inputs through the database: it still plays, with more input delay.
- An online fight cannot be paused. Closing the page or pressing Esc / the pause button and **Leave match** ends it.
- A win or a loss is recorded when a match finishes. The leaderboard shows the 50 players with the most wins.

## Looking after it

- **Remove a bad picture:** Firebase > Realtime Database > `ft` > `users` > that player > delete the `av` line. Their
  picture goes back to a default fighter. Reports from players appear under `ft` > `reports`.
- **Remove a player's scores or name:** delete their entry under `ft` > `users` (and their name under `ft` > `names`).
- **Limits:** Firebase's free plan has daily and monthly limits. A small game sits well inside them; if the game
  takes off, check the Usage tab now and then.
- **Scores are reported by the players' own devices.** The rules stop obvious tampering (a score can only go up by
  one at a time, and only its owner can change it), but a determined cheat could still inflate their own wins.
  Proper protection needs a server that referees matches; that is a later step if the leaderboard starts to matter.
- **Better connections on mobile data:** a "TURN" relay service makes direct links work on almost every network.
  If you sign up for one, its details go in `fb-config.js` as `window.FT_ICE = [ ... ];`.

## What is in this folder

| File | What it is |
| --- | --- |
| `index.html` | The game for computers (keyboard, gamepad) |
| `m.html` | The game for phones and tablets (touch controls, layout editor) |
| `preview.jpg` | The picture WhatsApp and other apps show when the link is shared |
| `favicon.ico`, `favicon.svg`, `icon-192.png`, `icon-512.png`, `apple-touch-icon.png`, `site.webmanifest` | The site icon, for browser tabs and phone home screens |
| `fb-config.js` | Your Firebase settings (already filled in) |
| `ft-rules-block.txt` | The rules block that was added to the database rules |

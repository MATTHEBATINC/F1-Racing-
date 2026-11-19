# F1 World Tour - online server

This folder is the game plus a small Node server that adds:

* **Live lobbies** - 2 to 6 people race at the same time (create a room, share the 4-letter code, host picks track / laps / dry or wet, everyone starts together).
* **World ghost** - the fastest lap for each track is stored on the server; turn on `GHOST: WORLD BEST` in the menu to race it.

The game itself is `public/index.html`. The server (`server.js`) serves that page, so players just open your Render address.

## 1. Put it on GitHub

1. Create a new empty repository on GitHub (private is fine).
2. In this folder run:
   ```
   git init
   git add .
   git commit -m "F1 World Tour online"
   git branch -M main
   git remote add origin https://github.com/YOUR-NAME/YOUR-REPO.git
   git push -u origin main
   ```

## 2. Deploy on Render

1. On https://render.com choose **New +  ->  Web Service** and connect the GitHub repository.
   (Or choose **New +  ->  Blueprint**: `render.yaml` in this folder already has the right settings.)
2. Settings if you fill them in by hand:
   * Runtime: **Node**
   * Build command: `npm install`
   * Start command: `npm start`
   * Health check path: `/health`
   * Plan: Free is enough
3. When the deploy finishes Render gives you an address like `https://f1-world-tour.onrender.com`.
   Open it - that is the game, with online racing enabled.

## 3. Race online

1. Everyone opens the **same Render address**.
2. In the menu press **ONLINE RACING**. The server field fills in automatically.
3. One person presses **CREATE ROOM** and shares the 4-letter code.
4. Everyone else types the code and presses **JOIN**.
5. The host chooses track, laps and dry/wet, then presses **START RACE**. Everyone gets the start lights at the same moment.
6. After the podium the room goes back to the lobby for another race.

Set your driver name, colours and number first in **CUSTOMISE** - other players see them.

## Things to know

* **Free Render services go to sleep** after about 15 minutes without traffic. The first visit afterwards takes up to a minute to wake the server. Paid plans stay awake.
* **Shared ghosts are kept in `ghosts.json` on the server.** On Render's free plan that file is wiped whenever the service restarts or redeploys. Add a Render Disk and set the data path to it if you want ghosts to last.
* **Cars race side by side but do not collide** online. Each player's game works out its own car and the server only passes the positions around, so it is simple and fast but it trusts every player. It is meant for friends, not for a public competition.
* **Weather online is fixed** (dry or wet, chosen by the host) so everybody sees the same race. AI cars are switched off in online races.
* The version of the game published on claude.ai cannot open connections to outside servers. Online racing only works from the Render address.

## Run it on your own computer first

```
npm install
npm start
```
Open http://localhost:3000 in two browser windows and join the same room to try it.

## Which files go in the repository

Put these in the **top level** of the GitHub repository, exactly like this:

```
server.js
package.json
render.yaml          (optional but handy)
.gitignore           (optional)
README.md            (optional)
public/
  index.html         <- the game (a folder called "public" with a file called "index.html")
```

Common mistakes:

* Uploading only the single game file (`F1Racing3D.html`). The server needs `server.js` and `package.json` too.
* Putting the game at the top level, or leaving it named `F1Racing3D.html`. It must be `public/index.html`.
* Putting `server.js` and `package.json` inside another folder. If you do, set **Root Directory** to that folder in Render.
* Uploading `node_modules` or `ghosts.json`. Leave those out; Render installs the packages itself.

## Checking that it works

1. In Render open the service and look at **Logs**. You want to see `F1 World Tour online server listening on port ...`.
2. Open `https://YOUR-ADDRESS.onrender.com/health` - it should show `ok`.
3. Open `https://YOUR-ADDRESS.onrender.com/` - that should show the game. If it shows a message about a missing game file, `public/index.html` is in the wrong place.
4. If the logs say the `ws` package is not installed, `package.json` is not next to `server.js`, or the build command is not `npm install`.

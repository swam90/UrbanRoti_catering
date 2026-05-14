# Urban Roti — Installable App

The Urban Roti app, packaged as an installable PWA. Host once, share the URL,
and your team adds it to their phone home screen on **Android** or **iOS**.
Works offline after first load.

---

## What's in this folder

| File | Purpose |
|---|---|
| `index.html` | The app itself |
| `manifest.webmanifest` | Tells phones it's an installable app |
| `sw.js` | Service worker — enables offline mode |
| `icon-192.png` / `icon-512.png` | App icons (standard) |
| `icon-512-maskable.png` | Android adaptive icon (handles circle/squircle crop) |
| `apple-touch-icon.png` | iOS home-screen icon (180×180) |
| `favicon.png` | Browser tab icon |

**Upload all of these together** to a web host with the filenames as-is.
The app won't install correctly if files are missing, renamed, or the host
isn't HTTPS.

---

## Step 1 — Host it (pick one, ~3 minutes)

### Option A: Netlify Drop (easiest, no signup to start)

1. Open **https://app.netlify.com/drop**
2. Drag this entire folder onto the page
3. You instantly get a URL like `https://lucky-name-12345.netlify.app`
4. Optional: sign up free to claim it permanently and rename to e.g. `urbanroti.netlify.app`

### Option B: Cloudflare Pages

1. Go to **https://pages.cloudflare.com** → sign up free
2. Click "Upload assets" → drag the folder
3. Get a URL like `https://urban-roti.pages.dev`

### Option C: GitHub Pages (most permanent)

1. Create a free GitHub account
2. Create a new repository named `urban-roti`, make it **public**
3. Upload all files in this folder to the repo root
4. Settings → Pages → Source: "Deploy from branch" → `main` / `/ (root)` → Save
5. After ~1 minute, your site is at `https://YOURNAME.github.io/urban-roti/`

> **Important:** the host **must use HTTPS**. All three options above do this
> automatically. PWAs won't install over plain `http://`.

---

## Step 2 — Share the URL with your team

Once hosted, send the URL via WhatsApp, email, or paste it into a group chat.
Below is the install message they need.

### For Android users

> Open this link in **Chrome**: `https://your-url-here`
> Chrome will show an "Install app" banner — tap it.
> If you don't see the banner, tap the **⋮ menu → Install app**.

### For iPhone / iPad users

> Open this link in **Safari** (not Chrome — iOS requires Safari for this):
> `https://your-url-here`
> Tap the **Share button** (square with up arrow) → scroll down →
> **Add to Home Screen** → Add.

After installing, the UR icon appears on the home screen and opens fullscreen
without browser bars — just like a real app from the Play Store / App Store.

---

## Step 3 — Updating the app later

When you change `index.html`:

1. Open `sw.js` and bump the version line:
   ```js
   const CACHE_VERSION = 'urban-roti-v1';   // change to 'urban-roti-v2', etc.
   ```
2. Re-upload the changed files to your host.
3. Users will pick up the new version next time they open the app
   (may need to close and reopen once for the service worker to refresh).

---

## Important limitations to share with your team

1. **Each phone stores its own data.** Quotations saved by Person A
   are not visible to Person B. The data lives in the browser's local storage
   on that one device. If a user clears their browser data or uninstalls,
   their history is gone.

2. **No central database.** If you need shared data across staff or devices,
   the app needs a real backend — that's a bigger build.

3. **No Play Store / App Store listing.** This installs via the browser. It
   looks and works like an app but doesn't appear in the stores. (Store listing
   needs extra wrapping — TWA for Play Store, Capacitor + a paid Apple Developer
   account for App Store.)

4. **iOS is stricter.** Some advanced PWA features (like push notifications)
   are limited on older iOS versions. Adding to home screen works since
   iOS 11.3 though, so install itself is fine.

5. **First open needs internet.** After that the app works offline thanks to
   the service worker, but the first visit must download the files
   (including Google Fonts and the PDF/Excel libraries from CDN).

---

## Troubleshooting

**"Install app" option doesn't appear on Android**
→ The site must be HTTPS (✓ if you used Netlify/Cloudflare/GitHub Pages).
→ Visit the page once, wait a few seconds, then check the ⋮ menu.

**iOS shows a blank icon on home screen**
→ Make sure `apple-touch-icon.png` is in the same folder as `index.html`
  and was uploaded together.

**"This site cannot provide a secure connection"**
→ Your host isn't HTTPS. Switch to one of the three options above.

**Users on different phones see different data**
→ Expected — see limitation #1 above. This app is single-device by design.

**App still shows the old version after I updated it**
→ Did you bump `CACHE_VERSION` in `sw.js`? If not, the service worker
  serves the cached old version. Bump the version, re-upload, then close and
  reopen the app on each device.

# Caption to Recipe PDF

A tiny installable web app. Paste (or share) a video's caption, it pulls out the
ingredients/steps, and generates a PDF — entirely in your phone's browser. No backend,
no AI model, no PC required.

## How it works

Most recipe captions already have structure ("Ingredients:", "Instructions:", bullet
points, numbered steps). The app just parses that structure with plain text rules —
no transcription, no AI call, works fully offline once loaded.

## 1. Deploy it (so your phone can install it as an app)

This needs to live at a real HTTPS address for "Add to Home Screen" and Android's
share-sheet integration to work. Free options, pick one:

**GitHub Pages** (free, easiest if you already use GitHub):
1. Create a new repo, upload all 4 files (`index.html`, `manifest.json`, `sw.js`, `icon.svg`) to the root.
2. Repo Settings → Pages → set source to the main branch.
3. Your app is live at `https://<username>.github.io/<repo>/`.

**Netlify** (free, no GitHub needed):
1. Go to [app.netlify.com/drop](https://app.netlify.com/drop).
2. Drag the folder with all 4 files onto the page.
3. It gives you a live HTTPS URL instantly.

## 2. Install it on your phone

- **Android (Chrome)**: open the URL, tap the menu → "Add to Home screen" / "Install app."
- **iPhone (Safari)**: open the URL, tap Share → "Add to Home Screen."

Once installed, it opens full-screen like a normal app.

## 3. Get it in the share sheet

- **Android**: after installing, your app should now appear as a share target. Open Instagram/Facebook/TikTok, tap Share on a post, and "Recipe PDF" should be in the list. Selecting it opens the app with the caption pre-filled.
- **iPhone**: iOS doesn't allow web apps to register as share targets. Use a **Shortcut** instead:
  1. Open the Shortcuts app → new Shortcut.
  2. Add action "Get Text from Input" (accepts Share Sheet input).
  3. Add action "Open URLs" with: `https://<your-deployed-url>/index.html?text=[Shortcut Input, URL-encoded]`
  4. In the Shortcut's settings, enable "Show in Share Sheet," and restrict input type to Text/URL.
  5. Now sharing a post → your Shortcut → opens the app with the caption pre-filled.

## Notes & limitations

- **Only works when the caption is actually shared/pasted with the video.** If someone shares just the video file with no caption text attached, there's nothing to parse — you'd need to manually copy the caption from the post and paste it in.
- **Parsing is rule-based, not AI.** It looks for "Ingredients"/"Instructions" headers, bullets, and numbers. Messy or very casual captions (no structure at all) may need manual cleanup in the edit boxes before generating the PDF — that's what the editable preview is for.
- **Fully offline after first load** — the service worker caches the app shell, and the jsPDF library is fetched from a CDN once and cached by the browser.

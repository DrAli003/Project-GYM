# 🏋️ The 10 kg Project — Recomp OS

An interactive, self-contained training + nutrition + progress tracker built for one person: 23 y, 85 kg → 75 kg, second-year medical student, shoulder and elbow sensitive, Mon–Fri training with weekends off.

**Upload this folder's `index.html` to GitHub Pages.** That single file *is* the whole app — no build step, no dependencies, no external requests, no tracking. Your data lives in your own browser (`localStorage`), never on a server.

---

## Publish it (5 minutes)

1. Create a new **public** repository on GitHub, e.g. `gym`.
2. Upload these files from this folder: **`index.html`** (the whole app — required), **`manifest.webmanifest`**, **`icon-192.png`**, **`icon-512.png`** (and `icon-512-maskable.png`) so Android/Chrome can install it with a proper icon, plus **`README.md`** if you want text shown on the repo page. *No folders, no build step.* Uploading only `index.html` still works — you'd just lose the Android install icon.
3. **Settings → Pages → Source: Deploy from a branch → Branch: `main` → `/ (root)` → Save.**
4. Wait ~60 seconds. Live at `https://<your-username>.github.io/gym/`
5. **Add it to your iPhone/iPad home screen** — instructions below; it then runs full-screen with its own icon, like a native app.

### Or from the command line

```bash
cd upload-to-github
git init && git add index.html README.md
git commit -m "Add 10 kg Project tracker"
git branch -M main
git remote add origin https://github.com/<your-username>/gym.git
git push -u origin main
```

Then enable Pages as in step 3.

---

## 📱 iPhone, iPad, Android phone & tablet

The app is one HTML file, so it runs in any modern mobile browser — and the mobile layout is driven by **screen width**, not by which OS you're on. Below is what is platform-specific.

### Android (Chrome / Edge / Samsung Internet)
- **Hardware + gesture back button works properly.** Back closes the **More** sheet if it's open, otherwise returns to the previous tab — it never dumps you out of the app on the first press. That required a small History-API state machine, and it's tested at three levels (sheet open, sheet navigation, plain tab change).
- **Install prompt:** on the Data tab there's an **📲 Install app** card that appears when Chrome offers installation (it's hidden on browsers that don't support it). Tapping it fires Chrome's native install dialog — no need to hunt through the ⋮ menu. `manifest.webmanifest` + the icons give it a proper launcher icon, full-screen display and a theme colour.
- **Gboard/keyboard:** focused fields get `scroll-margin` so the on-screen keyboard doesn't cover the box you're typing in, and the sheet traps overscroll so the page behind it doesn't scroll.
- **Chrome's font-boosting** (which inflates text in wide blocks and wrecks layouts) is disabled with `text-size-adjust: 100%`, tap highlights are styled rather than default grey, and `color-scheme` is declared so Chrome's own dark mode and form controls match the app theme.

### iPhone & iPad

**Add to Home Screen (recommended — makes it a proper app):**
- **iPhone/iPad Safari:** open your Pages link → tap the **Share** button (□ with ↑) → scroll down → **Add to Home Screen** → **Add**.
- It launches full-screen (no address bar), has its own dark-mode-aware icon, respects the notch/Dynamic Island and home-indicator safe areas, and works offline after the first load.

**What was done for iOS specifically:**
- **Bottom tab bar** with 5 thumb-reachable buttons (Today · Train · Progress · Food · More) — the full 7-section nav lives behind **More**, which opens as an iOS-style bottom sheet. On iPad it also appears in portrait; in landscape you get the full desktop nav.
- **No accidental zoom** when you tap a field: iOS zooms any input under 16 px, so all inputs are 16 px on iOS and the viewport is fixed (you can still pinch-zoom manually).
- **Correct keyboards**: number pad for steps/reps, decimal pad for weights and macros, "search" return key on the food search, autocorrect off there too.
- **44–52 px touch targets** on buttons, checkboxes and tabs (Apple's minimum), and `touch-action: manipulation` to kill the 300 ms tap delay.
- **Charts redraw** at a phone-friendly size and re-render when you rotate the device; wide tables scroll inside their own containers so the page never scrolls sideways.
- **Safe-area padding** top and bottom, toasts repositioned above the tab bar, and a responsive layout that goes single-column below 700 px and 2-up on iPad.
- Splash/status-bar meta tags so it looks right when launched from the home screen and in light *and* dark mode.

## What's inside

| Tab | What it does |
|---|---|
| **Today** | Today's session with set logging, morning weigh-in, macro quick-log, steps/sleep, 4-vs-5-day week switch, prehab block |
| **Train** | All five days with cues, "last time" reference and double-progression suggestions; log any date |
| **Progress** | 7-day-average weight chart with target path, waist chart, weekly volume, 20-week consistency heatmap, PR board |
| **Nutrition** | **220+ food database** (Lebanese dishes, mezze, manakish, snacks, chocolate, drinks, fruits, brand items), free-text smart parsing (`"2 manakish zaatar"`, `"150g chicken"`), your own custom foods, and an **optional AI estimator** |
| **Method** | The eight evidence-based rules, block plan, program comparison, whey & creatine deep-dives, references |
| **Shoulder & Elbows** | Working hypothesis, pain rules, every substitution and why, prehab, return-to-barbell ladder |
| **Data** | Profile & targets, AI key settings, backup/restore, CSV exports, 4-week summary, demo data, reset |

### Scheduling
**Saturday and Sunday are always rest.** 4-day week: Mon Upper A · Tue Lower A · Thu Upper B · Fri Lower B. 5-day week: Wednesday becomes the Delt & Arm Day. Switch it on the Today tab; a missed session slides into any free weekday.

### The AI food estimator (optional)
Describe a meal in words and get kcal + macros back. Bring your own API key — Google Gemini has a **free tier** (no credit card); OpenAI and Anthropic also work.

**Getting a Gemini key (2 minutes):**
1. Go to **aistudio.google.com** and sign in with your Google account.
2. Left sidebar → **Get API key** (direct link: `aistudio.google.com/app/apikey`).
3. **Create API key → Create key in new project.** Nothing else to configure.
4. Copy it (starts with `AIza`) → paste into **Data → AI nutrition estimator → API key** → **Save AI settings** → **Test connection**.

**If a model name ever fails:** press **Find available models** — the app lists every model your key can actually use and puts them in a dropdown. Model names are retired regularly (Google retired `gemini-2.0-flash` in June 2026), which is why the default is the moving alias `gemini-flash-latest`.

**Free-tier limits:** roughly 10–15 requests/minute and several hundred to 1,500/day per project — far more than a food tracker needs.

**Privacy:** your key lives **only in this browser's localStorage** and is never written into `index.html`, so it never reaches GitHub. Note that on Google's free tier, requests may be used to improve their models; on other providers' APIs they generally are not. Without a key at all, the **Copy prompt** button gives you the same result by pasting into ChatGPT or Claude manually.

## Privacy & backups
A public repo means the *site* is public; it contains no personal data. Your logs never leave the browser. Export a JSON backup from the Data tab after each weekly check-in — clearing browser data or switching devices loses the local copy.

## A caveat worth keeping
This is general training, nutrition and prehab guidance for one person — not a diagnosis. Night pain, weakness, numbness, swelling, or pain that hasn't improved after 6–8 weeks of load management needs a clinician who can examine you in person.

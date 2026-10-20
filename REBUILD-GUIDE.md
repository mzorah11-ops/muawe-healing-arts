# Rebuild the Muawé Healing Arts website on a new device

The whole website is saved on GitHub, so you can get it back exactly as it is. Follow the steps in order.

## 1. The short version (5 minutes)

1. Install **Node.js 18 or newer** from https://nodejs.org (the LTS button). This is only needed to run the site on your own computer.
2. Install **Git** from https://git-scm.com/downloads (accept the defaults).
3. Open a terminal (Windows: PowerShell. Mac: Terminal) and run:

```
git clone https://github.com/mzorah11-ops/muawe-healing-arts.git "AI Website Building"
cd "AI Website Building"
node serve.mjs
```

4. Open **http://localhost:3000** in a browser. That is the full site, identical to before.

No `npm install` and no build step. The site is one file (`index.html`) plus the `images/` folder.

If you do not want Git: go to https://github.com/mzorah11-ops/muawe-healing-arts, press the green **Code** button, then **Download ZIP**. Unzip it, open a terminal in that folder and run `node serve.mjs`.

## 2. Where everything is saved

| What | Where | Status |
| --- | --- | --- |
| The main website (`index.html`, `images/`, `emails/`, `brand_assets/`, `serve.mjs`, `README.md`) | https://github.com/mzorah11-ops/muawe-healing-arts (branch `main`) | Saved. Latest commit: "Make mobile menu links light so they read on the navy background" |
| The second design (sphere version) | https://github.com/mzorah11-ops/muawe-healing-arts-sphere | Saved. Clone it into a folder called `clone-site` if you want it back |
| This guide | `REBUILD-GUIDE.md` in the main repository | Saved when committed and pushed |
| Working files that are **not** on GitHub (kept out on purpose) | Only on the old computer | Listed in section 5. Everything that matters from them is written out in this guide |

Your project folder is also inside OneDrive (Desktop). If OneDrive finished syncing, the whole folder, including the working files, will also appear in OneDrive on the new device.

## 3. What the site is

- A one-page site for Muawé Healing Arts, an art psychotherapy practice in Manchester City Centre.
- Sections: hero, About, Services (with prices and detail pages), art for sale, reviews, Gallery, Journal, Visit (map), booking form and footer.
- Plain HTML, CSS and JavaScript in one file. Nothing to build.
- Brand colours: navy `#071B4D`, secondary navy `#0C2766`, gold/orange `#FF9A00`, sand `#F6F3EC`, slate `#5B6B8A`, charcoal `#1A1A1A`. The page is mostly ivory with navy and gold accents.
- Fonts (loaded from Google Fonts, so the new device needs internet): **Playfair Display** (headings), **Montserrat** (body), **Antonio**.
- Hero background, the About section's moving light (WebGL) and the floating paintings are drawn in the browser. They switch off for visitors who prefer reduced motion.

## 4. How booking works (the one thing that lives outside the code)

- The booking form sends each request to a **Zapier webhook**. The address is the `WEBHOOK` constant near line 1218 of `index.html`, so it is already saved in the repository.
- **The Zap itself is stored in your Zapier account, not in the code.** Log in to Zapier from the new device to see or edit it. It runs the reservation flow and sends the confirmation email.
- The email template is `emails/reservation-confirmation.html`. It has `{{placeholders}}` for Zapier. The Zap needs a Formatter step to turn the date and time into friendly text (see the comments in the template).
- Fields sent: `name`, `email`, `booking_type`, `service`, `preferred_date` (YYYY-MM-DD), `preferred_time` (HH:MM, 24-hour), `message`, `source`, `page`, `submitted_at`.
- The form says "Thank you. Your booking is confirmed." only after Zapier replies with success. On failure it shows an error and the practice's email address.
- Booking hours: 9am to 8pm, in 15-minute slots.
- The repository is public, so the webhook address is visible. Keep a filter step at the start of the Zap (for example, only continue if the email looks real).

## 5. Files that were NOT put on GitHub, and what to do about them

These are in `.gitignore`. They are tools and working files, not part of the website, so the site works without them.

| File or folder | What it was | What to do |
| --- | --- | --- |
| `CLAUDE (1).md` | Rules that tell Claude Code how to build in this project | Full text is in Appendix A. Save it as `CLAUDE.md` in the project folder on the new device |
| `screenshot.mjs` | Takes a screenshot of the site with Puppeteer | Full text is in Appendix B |
| `temporary screenshots/` | Old test screenshots | Not needed. Take new ones |
| `.agents/`, `.taste-skill/`, `skills-lock.json` | Design skills installed for Claude Code (from `emilkowalski/skills` on GitHub and others) | Reinstall the skills if you want them. Not needed to run the site |
| `.claude/` | Claude Code settings for this project | Not needed. Claude Code recreates it |
| `brand_assets/houmahani.com_.png` | A screenshot of another person's website, used only as a design reference | Deliberately not published. Skip it |
| `clone-site/` | The second design, which has its own repository | Clone it from the link in section 2 if you want it |

## 6. Carrying on with Claude Code on the new device

1. Install Claude Code from https://claude.com/claude-code and sign in with the same account.
2. Open the cloned project folder in VS Code and start Claude Code there.
3. Put `CLAUDE.md` (Appendix A) in the project root.
4. Start the server with `node serve.mjs` and open http://localhost:3000.
5. To take screenshots: install Puppeteer (`npm install puppeteer` in a separate folder, for example `puppeteer-test`), put `screenshot.mjs` (Appendix B) in the project root and set the `PUPPETEER_DIR` environment variable to that folder.
6. To save changes: `git add .`, `git commit -m "your message"`, `git push`. You must be signed in to GitHub as `mzorah11-ops` on the new device (Git will open a browser window to sign in the first time).

## 7. If something does not look right

- **Fonts look different:** you are offline. Fonts come from Google Fonts.
- **Blank page or "Not found":** you ran `node serve.mjs` from the wrong folder. Run it from the folder that contains `index.html`.
- **Port 3000 in use:** close the other program using it, or change `3000` in `serve.mjs`.
- **Images missing:** make sure the `images/` folder came with the clone or ZIP.
- **Check you have the latest version:** run `git log --oneline -3`. The top line should read "Make mobile menu links light so they read on the navy background" or something newer.

---

## Appendix A: Claude Code rules (the old `CLAUDE (1).md`)

```markdown
# CLAUDE.md — Frontend Website Rules

## Always Do First
- **Invoke the `frontend-design` skill** before writing any frontend code, every session, no exceptions.

## Reference Images
- If a reference image is provided: match layout, spacing, typography, and color exactly. Swap in placeholder content (images via `https://placehold.co/`, generic copy). Do not improve or add to the design.
- If no reference image: design from scratch with high craft (see guardrails below).
- Screenshot your output, compare against reference, fix mismatches, re-screenshot. Do at least 2 comparison rounds. Stop only when no visible differences remain or user says so.

## Local Server
- **Always serve on localhost** — never screenshot a `file:///` URL.
- Start the dev server: `node serve.mjs` (serves the project root at `http://localhost:3000`)
- `serve.mjs` lives in the project root. Start it in the background before taking any screenshots.
- If the server is already running, do not start a second instance.

## Screenshot Workflow
- Puppeteer is installed at `C:/Users/User/puppeteer-test/`. Chrome cache is at `C:/Users/User/.cache/puppeteer/`.
- **Always screenshot from localhost:** `node screenshot.mjs http://localhost:3000`
- Screenshots are saved automatically to `./temporary screenshots/screenshot-N.png` (auto-incremented, never overwritten).
- Optional label suffix: `node screenshot.mjs http://localhost:3000 label` → saves as `screenshot-N-label.png`
- `screenshot.mjs` lives in the project root. Use it as-is.
- After screenshotting, read the PNG from `temporary screenshots/` with the Read tool — Claude can see and analyze the image directly.
- When comparing, be specific: "heading is 32px but reference shows ~24px", "card gap is 16px but should be 24px"
- Check: spacing/padding, font size/weight/line-height, colors (exact hex), alignment, border-radius, shadows, image sizing

## Output Defaults
- Single `index.html` file, all styles inline, unless user says otherwise
- Tailwind CSS via CDN: `<script src="https://cdn.tailwindcss.com"></script>`
- Placeholder images: `https://placehold.co/WIDTHxHEIGHT`
- Mobile-first responsive

## Brand Assets
- Always check the `brand_assets/` folder before designing. It may contain logos, color guides, style guides, or images.
- If assets exist there, use them. Do not use placeholders where real assets are available.
- If a logo is present, use it. If a color palette is defined, use those exact values — do not invent brand colors.

## Anti-Generic Guardrails
- **Colors:** Never use default Tailwind palette (indigo-500, blue-600, etc.). Pick a custom brand color and derive from it.
- **Shadows:** Never use flat `shadow-md`. Use layered, color-tinted shadows with low opacity.
- **Typography:** Never use the same font for headings and body. Pair a display/serif with a clean sans. Apply tight tracking (`-0.03em`) on large headings, generous line-height (`1.7`) on body.
- **Gradients:** Layer multiple radial gradients. Add grain/texture via SVG noise filter for depth.
- **Animations:** Only animate `transform` and `opacity`. Never `transition-all`. Use spring-style easing.
- **Interactive states:** Every clickable element needs hover, focus-visible, and active states. No exceptions.
- **Images:** Add a gradient overlay (`bg-gradient-to-t from-black/60`) and a color treatment layer with `mix-blend-multiply`.
- **Spacing:** Use intentional, consistent spacing tokens — not random Tailwind steps.
- **Depth:** Surfaces should have a layering system (base → elevated → floating), not all sit at the same z-plane.

## Hard Rules
- Do not add sections, features, or content not in the reference
- Do not "improve" a reference design — match it
- Do not stop after one screenshot pass
- Do not use `transition-all`
- Do not use default Tailwind blue/indigo as primary color
```

## Appendix B: `screenshot.mjs`

```js
// Usage: node screenshot.mjs http://localhost:3000 [label] [width] [height]
// Puppeteer is installed in C:/Users/User/puppeteer-test/ (override with the PUPPETEER_DIR env var).
import { mkdirSync, readdirSync } from 'node:fs';
import { createRequire } from 'node:module';
import { join } from 'node:path';

const require = createRequire(join(process.env.PUPPETEER_DIR || 'C:/Users/User/puppeteer-test', '/') + 'x.js');
const puppeteer = require('puppeteer');

const [url = 'http://localhost:3000', label = '', w = '1440', h = '900'] = process.argv.slice(2);
const dir = 'temporary screenshots';
mkdirSync(dir, { recursive: true });
const n = readdirSync(dir).filter(f => /^screenshot-\d+/.test(f)).length + 1;
const out = join(dir, `screenshot-${n}${label ? '-' + label : ''}.png`);

const browser = await puppeteer.launch();
const page = await browser.newPage();
await page.setViewport({ width: +w, height: +h });
await page.goto(url, { waitUntil: 'networkidle0' });
await new Promise(r => setTimeout(r, 1800));
// scroll through the page so reveal animations fire
await page.evaluate(async () => { document.documentElement.style.scrollBehavior = 'auto'; for (let y = 0; y < document.body.scrollHeight; y += 300) { window.scrollTo(0, y); await new Promise(r => setTimeout(r, 300)); } window.scrollTo(0, 0); });
await new Promise(r => setTimeout(r, 800));
await page.screenshot({ path: out, fullPage: true });
await browser.close();
console.log(out);
```

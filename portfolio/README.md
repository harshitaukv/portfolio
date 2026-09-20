# Harshita U — Portfolio

A single-file static site. `index.html` is the whole thing: the CSS, the
JavaScript, the fonts link and the photograph (embedded as a data URI) all live
inside it. Nothing to install, nothing to build.

```
portfolio/
├── index.html      the site
├── harshita.jpg    the source photograph, kept so it can be swapped
└── vercel.json     caching headers (optional)
```

---

## Deploy on Vercel

### Option A — drag and drop (fastest, no Git)

1. Go to **vercel.com/new**
2. Drag this whole folder onto the page
3. Framework Preset: **Other**. Leave Build Command and Output Directory empty.
4. **Deploy**

Live in about thirty seconds.

### Option B — from GitHub (recommended, so edits redeploy themselves)

```bash
cd portfolio
git init
git add .
git commit -m "Portfolio"
git branch -M main
git remote add origin https://github.com/harshitaukv/portfolio.git
git push -u origin main --force
```

Then on **vercel.com/new** → *Import Git Repository* → pick `portfolio` →
Framework Preset **Other** → **Deploy**.

Every `git push` after that redeploys automatically.

> You already have a repository called `portfolio`. `--force` overwrites what is
> in it. Drop the flag and resolve the merge if you want to keep its history.

### Option C — Vercel CLI

```bash
npm i -g vercel
cd portfolio
vercel --prod
```

---

## Custom domain

In the Vercel dashboard: **Settings → Domains → Add**. Vercel issues the HTTPS
certificate itself; there is nothing to configure on this end.

---

## Editing

Everything is in `index.html`, in reading order.

| To change | Look for |
| --- | --- |
| Colours | the `:root` block at the top — `--violet`, `--coral`, `--amber`, `--cyan`, `--green`, `--indigo` are the spectrum |
| Fonts | the `<link>` to Google Fonts, then `--f-display` / `--f-body` / `--f-mono` |
| Any text | the HTML below `<div class="shell">`, one `<section>` per heading |
| Which hue a section uses | the `/* per-section hue */` block — one line per section |
| Animation | the `<script>` at the bottom: scroll meter, reveals, counting stats, card tilt |

### Swapping the photograph

The photo is embedded as a base64 data URI so the site stays one file. To
replace it:

```bash
# macOS / Linux
base64 -w0 new-photo.jpg > photo.txt

# Windows PowerShell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("new-photo.jpg")) | Set-Content photo.txt
```

Then in `index.html` find `src="data:image/jpeg;base64,` and replace everything
between that comma and the closing quote with the contents of `photo.txt`.

Keep the new photo small — around 400×520 and under 60 KB. It is embedded in the
HTML, so its size is added to the page's download.

### Turning the background colour fields off

Delete the two `<div class="aurora">` blocks. Nothing else depends on them.

---

## Notes

- Every animation respects `prefers-reduced-motion`: a visitor whose system asks
  for reduced motion gets the page with the drifting colour, the reveals, the
  counting numbers and the card tilt all switched off.
- The page works with JavaScript disabled — every section is visible and the
  statistics still read correctly, because the numbers are written into the HTML
  and the counter only animates up to them. Only the scroll meter, the reveals
  and the pointer tilt are lost.
- Colour carries meaning here: each of her four project domains owns a hue, and
  that hue follows the work through its section rule, its card glow and its tag.
- Google Fonts is the one external request. If it fails, the page falls back to
  Georgia and a system monospace, and the layout is unchanged.

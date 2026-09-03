# sunkaiwen.com

Personal portfolio — hand-built, no framework, no build step. One HTML file plus an images folder.

## Part 1 — Get it on GitHub

The repo name does **not** matter here, because the site will live at your own domain. `portfolio` is a fine name.

1. github.com → New repository → name it `portfolio`, **Public**, don't add a README.
2. Click *uploading an existing file* and drag in `index.html`, `README.md`, `CNAME`, and the `images` folder. They must sit at the **top level** of the repo — if `index.html` ends up inside a subfolder, nothing works.
3. Commit.
4. Settings → Pages → Source: **Deploy from a branch**, Branch: `main`, folder: `/ (root)`. Save.
5. Wait a minute. It goes live at `https://YOURUSERNAME.github.io/portfolio` — check it works there first, before touching DNS.

Every push to `main` republishes automatically.

## Part 2 — Point sunkaiwen.com at it

The `CNAME` file in this repo already contains `sunkaiwen.com`, which is what tells GitHub the domain is yours. You just need the DNS side.

### Add these records at whoever manages your domain

If you bought the domain through Squarespace: **Settings → Domains → sunkaiwen.com → DNS Settings**. Otherwise it's the DNS panel at your registrar.

Delete any existing A records or `www` CNAME pointing at Squarespace, then add:

| Type | Host / Name | Value |
|---|---|---|
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| CNAME | `www` | `YOURUSERNAME.github.io` |

Optional but recommended — IPv6, same `@` host:

```
AAAA  @  2606:50c0:8000::153
AAAA  @  2606:50c0:8001::153
AAAA  @  2606:50c0:8002::153
AAAA  @  2606:50c0:8003::153
```

Some panels use a blank host instead of `@` for the root domain. The `www` CNAME value must end in `.github.io` — not the repo name, not the full site URL.

### Then, back on GitHub

Settings → Pages → Custom domain → enter `sunkaiwen.com` → Save. Wait for the green "DNS check successful", then tick **Enforce HTTPS** (the certificate can take up to an hour to issue — the tickbox stays greyed out until it's ready).

Once both are configured, `www.sunkaiwen.com` redirects to `sunkaiwen.com` automatically, so the URL printed on your résumé keeps working.

### Timing and rollback

DNS changes typically propagate in minutes but can take up to 48 hours. Your Squarespace site stays reachable at its built-in `*.squarespace.com` address the whole time, so nothing is lost — and if something goes wrong you can put the old records back.

**Before cancelling Squarespace:** if the domain came bundled with your plan, check whether cancelling forfeits it. You may need to pay for the domain separately or transfer it to a registrar like Cloudflare or Namecheap first. Do that before you cancel, not after.

## Where to edit

Open `index.html`, scroll to the `<script>` tag. Two objects control the whole site.

| What | Where |
|---|---|
| Name, hero statement, availability line, bio, education, experience, skills | `PROFILE` |
| Every project | `PROJECTS` |

The hero `statement` is a three-part array — the middle string gets the mint highlight.

### A project entry

```js
{
  id:"section-automation",          // URL: #/work/section-automation
  cat:"Computation",                // "Computation" or "Architecture" — drives the Field filter
  title:"Section Automation Plugin",
  year:"2026",
  image:"images/thing.jpg",         // "" falls back to a generated placeholder
  images:["images/a.jpg","images/b.jpg"],  // optional gallery below the text
  tags:["Automation","C#"],         // tags populate the Method filter
  sub:"One line under the title.",
  meta:{Role:"…",Context:"…",Stack:"…",Duration:"…"},  // any keys you like
  wip:true,                         // optional — shows an "In progress" chip
  blocks:[{tag:"The problem", h:"Headline", p:"Paragraph."}]  // empty [] → "case study in progress" note
}
```

Numbering (`C01`, `A03`) is generated from category + position — reorder the array and it renumbers itself.

## Content status

**Written from your résumé and design portfolio PDF.** Verify everything before shipping — I wrote the project narratives from your own descriptions, but the voice is mine, not yours.

**Images already extracted from your portfolio PDF** (1600×900, web-optimized):

| Project | Files |
|---|---|
| Compression-Only Funicular Bench | `bench`, `bench-form`, `bench-robot`, `bench-matrix`, `bench-leg` |
| Future-Mobility Innovation Park | `innovation-park` |
| Boston Public Library | `boston-library` |
| Uptown Fire Station | `fire-station`, `fire-station-bay` |
| Delta Visitor Center | `delta-visitor`, `delta-visitor-2` |
| Pittsburgh Necklace | `pittsburgh-necklace`, `pittsburgh-necklace-2` |
| All About Bamboo | `bamboo` |

Also extracted but unused: `bridge-optimization`, `detail-studies` (the CMU pedestrian bridge and detail studies from your "Other Work" pages) — add them as projects if you want them.

**Still needs images** — these six run on generated placeholders:

- Section Automation Plugin — a screenshot of a generated sheet set would be the strongest single image on the site
- Gesture-to-Robot Collaborative Drawing — a photo or video still of the arm mid-draw
- Real-time Material Recognition — an annotated detection frame
- Tangible Telepresence — needs both images and a write-up
- Perspective Rectifier — needs both
- 2.5D Robotic Drawing — needs both

## Before you ship

- [ ] Verify every project description — especially dates, collaborators, and instructor names
- [ ] Confirm the availability line ("Graduating May 2027") says what you want recruiters to read
- [ ] Add images for the six computation projects
- [ ] Write up the three `wip:true` projects, then delete the `wip` flag
- [ ] Add `resume.pdf` and `portfolio.pdf` to the repo; set `cv:"resume.pdf"` in `PROFILE` and fix the footer links
- [ ] Fill in the LinkedIn and GitHub URLs in the footer HTML
- [ ] Decide whether to list your phone number (it's on your résumé, not on the site)
- [ ] Delete the mint "Before you ship" note in the footer

## Design notes

- **Type** — Archivo (display + UI) with IBM Plex Mono for labels, metadata and numerals. Google Fonts, with system fallbacks.
- **Color** — white ground, green-biased neutrals, `#52F7A8` as the single accent. Mint is used as a *fill* only; accent text uses `--mint-deep` (`#067A45`) so it stays legible on white.
- **Cursor** — a lerping ring that fills mint over links and expands to a "View" label over project rows, with a hover preview panel trailing behind it. Disabled on touch and under `prefers-reduced-motion`.
- **Structure** — Computation leads the homepage, Architecture follows. The Field/Method filter pair on the Work page is the one bit of taxonomy on the site; it exists because your work genuinely splits two ways.
- **Routing** — hash-based (`#/work/bench`), which is why this is one file with no server config. Deep links and the back button both work.

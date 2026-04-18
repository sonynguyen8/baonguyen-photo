---
name: add-portfolio
description: Detect new portfolio image folders and scaffold the corresponding detail page and index entry. Use when the user adds new images to public/images/portfolios/ and wants a new portfolio page generated.
---

# Add Portfolio Skill

## Purpose

Compare image folders in `public/images/portfolios/` against existing pages in `src/pages/portfolios/`. For each unmatched folder, ask the user for routing details and generate the full detail page plus the `index.astro` entry.

---

## Step 1 — Detect new folders

Run both of these and compare:

```bash
# All image folders (one per shoot)
ls public/images/portfolios/Pre-Wedding/

# All existing detail pages (exclude index and [slug])
ls src/pages/portfolios/ | grep -v '^index\.' | grep -v '^\[slug\]'
```

Map folder names to expected page slugs using this convention:
- `J-M_pre-wedding` → `j-m-pre-wedding.astro`
- `Q-A_pre-wedding` → `q-a-pre-wedding.astro`

Report which folders have no matching page. If all folders are covered, tell the user and stop.

---

## Step 2 — Ask the user (once per new folder)

For each unmatched folder, ask these questions:

1. **Route slug** — the URL segment, e.g. `j-m-pre-wedding` (becomes `/portfolios/j-m-pre-wedding`)
2. **Couple display name** — shown in the `<h1>`, e.g. `J & M`
3. **Category line** — shown in `.pf-meta`, e.g. `Pre-Wedding`
4. **Description** — the atmospheric subtitle under the title (1–2 sentences)
5. **Thumbnail image** — filename for `portfolios/index.astro` (default: first file in folder)
6. **Display title** for `index.astro` entry, e.g. `Jack & Minh - Prewedding`
7. **Category filter** for `index.astro` — one of: `Portraits`, `Love Stories`, `Weddings`
8. **Year label** for `index.astro`, e.g. `01-2025`

---

## Step 3 — List images in the folder

```bash
ls public/images/portfolios/Pre-Wedding/<FOLDER_NAME>/
```

Check for unsafe filenames (containing `&` or non-ASCII characters). If found:
- Warn the user
- Suggest safe renames (e.g. `Q&A1.jpg` → `QA1.jpg`)
- Wait for confirmation before renaming

Build the `imgs` array from the sorted file list.

---

## Step 4 — Generate the detail page

Create `src/pages/portfolios/<slug>.astro` using this exact template structure (based on `j-m-pre-wedding.astro`):

```astro
---
import Base from "../../layouts/Base.astro";
import { Image } from "astro:assets";
const bp = "/images/portfolios/<Category>/<FOLDER_NAME>";
const imgs = [
  "file1.jpg","file2.jpg", ...
].map(f => `${bp}/${f}`);
---
<Base title="<CoupleDisplayName> — <Category>" description="<Description>">

<div class="cinematic-scrubber" id="scrubber"></div>

<main class="pf-page">

  <header class="pf-header">
    <a href="/portfolios" class="back-link">
      <span class="material-symbols-outlined">arrow_back</span>
      <span>Back to Portfolios</span>
    </a>
    <h1 class="pf-title"><CoupleDisplayName><br/><Category></h1>
    <p class="pf-desc"><Description></p>
    <div class="pf-meta">
      <span><Category></span>
      <span class="pf-meta-line"></span>
      <span><N> Plates</span>
    </div>
  </header>

  <div class="gal">
    <!-- S1: Hero + 2 stacked (001–003) — always rendered -->
    <!-- S2–S8: wrapped in {imgs[N] && ...} guards -->
    <!-- See docs/PORTFOLIOS.md for section guard table -->
  </div>

  <section class="pf-cta">
    <h2>Capture your narrative.</h2>
    <a href="/contact" class="pf-cta-btn">Book a Session</a>
  </section>

</main>
</Base>
```

Copy the full `<script>` and `<style>` blocks verbatim from `j-m-pre-wedding.astro` — they are identical across all pages.

Apply the section guards from `docs/PORTFOLIOS.md`:
- If `imgs.length >= 4` → include S2
- If `imgs.length >= 7` → include S3
- If `imgs.length >= 11` → include S4
- If `imgs.length >= 15` → include S5
- If `imgs.length >= 19` → include S6
- If `imgs.length >= 21` → include S7
- If `imgs.length >= 22` → include S8 (`.map()` grid for all remaining images)
- If gallery ends at 20–21 images → use `.sout` outro instead of S8

---

## Step 5 — Update portfolios/index.astro

Add the new entry to the `works` array in `src/pages/portfolios/index.astro`:

```js
{
  title: "<DisplayTitle>",
  category: "<CategoryFilter>",
  year: "<YearLabel>",
  url: "/<slug>",
  src: "/images/portfolios/<Category>/<FOLDER_NAME>/<ThumbnailFile>",
},
```

Insert it in chronological order (by `year` label) or at the end if unclear.

---

## Step 6 — Confirm

Tell the user:
- File created: `src/pages/portfolios/<slug>.astro`
- Entry added: `portfolios/index.astro`
- Remind them to run `npx emdash dev` (with Node v22) to preview

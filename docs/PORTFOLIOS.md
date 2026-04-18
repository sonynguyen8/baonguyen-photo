# Portfolio Feature Documentation

## Overview

Portfolios are statically-authored Astro pages. Each shoot has:
- An image folder under `public/images/portfolios/<Category>/<Folder>/`
- A detail page at `src/pages/portfolios/<slug>.astro`
- A thumbnail entry in `src/pages/portfolios/index.astro`

---

## File & Folder Structure

```
public/
└── images/
    └── portfolios/
        └── Pre-Wedding/               ← category folder
            └── J-M_pre-wedding/       ← shoot folder (images live here)
                ├── DSC_6235.jpg
                ├── DSC_6354.jpg
                └── ...

src/
└── pages/
    └── portfolios/
        ├── index.astro                ← grid listing all portfolios
        ├── j-m-pre-wedding.astro      ← detail page (slug = j-m-pre-wedding)
        └── ...
```

### Naming convention

| Thing | Convention | Example |
|---|---|---|
| Image folder | `INITIALS_category` | `J-M_pre-wedding` |
| Page slug | `initials-category` | `j-m-pre-wedding` |
| URL | `/portfolios/<slug>` | `/portfolios/j-m-pre-wedding` |

---

## Detail Page Structure

Every detail page (`*.astro`) follows the same template. Key parts:

### Frontmatter

```astro
---
import Base from "../../layouts/Base.astro";
import { Image } from "astro:assets";

const bp = "/images/portfolios/Pre-Wedding/J-M_pre-wedding";
const imgs = [
  "DSC_6235.jpg", "DSC_6354.jpg", ...
].map(f => `${bp}/${f}`);
---
```

- `bp` — base path to the image folder (must match `public/` path exactly)
- `imgs` — hardcoded array of filenames in display order, mapped to full paths
- **Do not use `readdirSync` or `existsSync`** — Cloudflare Workers runtime does not have filesystem access at runtime

### Section Layout

Pages are divided into cinematic sections. Each section has a minimum-image guard so pages with fewer images still render correctly.

| Section | Class | Images used | Guard |
|---|---|---|---|
| S1 | `.s1` | 001–003 | always rendered |
| S2 | `.s2` | 004 | `{imgs[3] && ...}` |
| S3 | `.s3` | 005–007 | `{imgs[6] && ...}` |
| S4 | `.s4` | 008–011 | `{imgs[10] && ...}` |
| S5 | `.s5` | 012–015 | `{imgs[14] && ...}` |
| S6 | `.s6` | 016–019 | `{imgs[18] && ...}` |
| S7 | `.s7` | 020–021 | `{imgs[20] && ...}` |
| S8 | `.s8` | 022+ (dynamic grid) | `{imgs[21] && ...}` |
| sout | `.sout` | final wide outro | alternative to S8 for short galleries |

**Minimum images to fill all sections: 22.** Galleries with fewer images simply skip the later sections.

### `<Image>` component

All images use Astro's `<Image>` with `layout="none"` to prevent conflict with the global `image: { layout: "constrained" }` in `astro.config.mjs`:

```astro
<Image src={imgs[0]} alt="Plate 001" width={1600} height={900} loading="lazy" layout="none" />
```

S8 grid thumbnails use `width={600} height={600}`.

---

## portfolios/index.astro — Thumbnail Grid

Add a new entry to the `works` array:

```js
{
  title: "Jack & Minh - Prewedding",   // display title
  category: "Weddings",                 // filter pill category
  year: "01-2025",                      // shown as label
  url: "/j-m-pre-wedding",             // appended to /portfolios
  src: "/images/portfolios/Pre-Wedding/J-M_pre-wedding/DSC_6235.jpg", // thumbnail
},
```

**Available categories:** `Portraits`, `Love Stories`, `Weddings`

---

## Adding a New Portfolio

### Quick steps

1. Drop images into `public/images/portfolios/<Category>/<Folder>/`
2. Rename any files with special characters (`&`, Unicode) to safe ASCII names
3. Run `/add-portfolio` in Claude Code — it will detect the new folder and scaffold the page
4. Review the generated page and adjust image order / section choice if needed
5. Add the entry to `portfolios/index.astro` (the command does this automatically)

### Filename safety rules

- No `&` in filenames (breaks URL encoding) → replace with nothing: `Q&A1.jpg` → `QA1.jpg`
- No Unicode/bold characters → rename to plain ASCII
- Spaces are allowed but discouraged → prefer `_` or `-`

---

## Image Guidelines

| Role | Recommended ratio | Notes |
|---|---|---|
| Hero / Opening (S1 main) | 16:9 | Landscape, strong composition |
| S1 side stack | 1:1 and 3:4 | Square + portrait pair |
| S2 full-width | any | Height capped at 60vh |
| S3 triptych | 2:3 | Three portrait columns |
| S4 tall left | 4:5 | Offset tall portrait |
| S8 grid | any | Cropped square in display |
| Thumbnail (index page) | 4:5 | 700×875px |

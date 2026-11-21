# Speak With Confidence — sales page

A single, self-contained landing page for the 8-week live English communication cohort.
Plain HTML, CSS and a small amount of vanilla JavaScript. No build step, no frameworks,
no external requests except the payment link.

```
/index.html                     all markup, inline CSS and inline JS
/fonts/lora-700-latin.woff2     Lora 700 (headings)
/fonts/inter-var-latin.woff2    Inter 400–700 (body, buttons, UI)
/fonts/inter-naira.woff2        1.2 KB subset containing only ₦ (U+20A6)
/images/instructor.webp         800×1000 WebP (hero + about, hero preloaded)
/images/instructor-480.webp     480×600 WebP (mobile srcset)
/images/instructor-placeholder.svg   4:5 SVG placeholder (kept for reference)
/images/og-placeholder.png     1200×630 link-preview image
/copy/sales-letter.md           source copy (authoritative — do not edit the page by hand)
/README.md                      this file
```

## Before you go live — the 4 things to replace

### 1. The payment link (required)

Every button points at the placeholder `__FLUTTERWAVE_PAYMENT_LINK__`.
Open `index.html` and replace **all 5 occurrences** with your real Flutterwave payment link.

- In VS Code: `Ctrl+H`, search `__FLUTTERWAVE_PAYMENT_LINK__`, replace with e.g.
  `https://flutterwave.com/pay/your-link`, then **Replace All**.
- All five buttons must point to the same link. The buttons are marked with
  `data-cta="hero"`, `features`, `investment`, `how-it-works` and `sticky`.

The page only ever contains a **public payment link**. Never put a secret key here.

### 2. Your name (required)

Replace `[TUTOR NAME]` (2 occurrences: the footer and the two photo `alt` texts).

### 3. Your photo

The page now uses a **srcset** with two WebP files for responsive images.
Prepare a source photo (4:5 portrait, face centred in the upper half with breathing room),
then generate two optimised WebP versions:

- **images/instructor.webp** — 800 × 1000 px, WebP, quality ~80, **under 150 KB**
- **images/instructor-480.webp** — 480 × 600 px, WebP, quality ~80, **under 70 KB**

Do not stretch or squash the image — crop to 4:5 if needed. Keep skin tones natural;
avoid over-compression (no blurry, blocky or washed-out results).

The `index.html` already references both files via `srcset` on the two `<img>` tags
(hero and "Why learn from me?"). Both tags keep `width="800" height="1000"` for
layout stability and `alt="[TUTOR NAME], English language tutor"`.

The hero image is preloaded (`<link rel="preload" as="image" href="images/instructor.webp">`)
and uses `fetchpriority="high" decoding="async"`. The about-section image uses
`loading="lazy" decoding="async"`. Both use `object-fit: cover; object-position: center 20%;`
so the face stays visible when cropped on narrow screens.

**To replace the photo in future:**
1. Drop your new source image in the folder (e.g. `images/tutor-new.jpg`).
2. Generate the two WebP files at the sizes above (any tool: sharp, cwebp, ImageMagick,
   Pillow, online converter — quality ~80, method 6).
3. Overwrite `images/instructor.webp` and `images/instructor-480.webp`.
4. No HTML changes needed — the `srcset` and `sizes` attributes handle the rest.

### 4. Link preview image

`images/og-placeholder.png` is a plain navy placeholder. Replace it with a 1200×630 image
if you want a branded preview. **After deploying**, change the two `og:image` / `twitter:image`
tags to the full absolute URL (e.g. `https://yourdomain.com/images/og-placeholder.png`) —
WhatsApp and TikTok need an absolute URL for previews.

## Turning on testimonials (only when you have real ones)

The testimonials section is in `index.html` with the `hidden` attribute, so it is invisible.
It contains three empty cards and **no invented text**.

1. Find the section that starts `<section class="section" id="testimonials" ... hidden>`.
2. Delete the word `hidden` from that tag.
3. Fill each card:

```html
<figure class="testimonial">
  <div class="t-avatar" aria-hidden="true"></div>
  <blockquote class="t-quote">Their words, exactly as they said them.</blockquote>
  <figcaption class="t-name">Name, role</figcaption>
</figure>
```

Add or remove `<figure>` blocks as needed (3–6 is recommended). Only use testimonials you
have permission to publish, and never present exam-prep results as communication-coaching results.

## Adding analytics

In `<head>` there is a clearly marked `ANALYTICS SLOT` comment. Paste your snippet
(Google Analytics, Meta Pixel, TikTok Pixel) directly beneath it. Nothing loads by default.
If you add a pixel, update your privacy notice accordingly.

## Deploying

The site is fully static — any host works. Pick one:

**Cloudflare Pages**
1. Push this folder to a GitHub repository.
2. Cloudflare dashboard → Workers & Pages → Create → Pages → Connect to Git.
3. Build command: *(leave empty)*. Output directory: `/` (or the folder containing `index.html`).
4. Deploy. Add your custom domain under Custom domains.

**GitHub Pages**
1. Push the folder to a repository (keep the `fonts/`, `images/` and `copy/` folder names —
   GitHub Pages is case-sensitive).
2. Repository → Settings → Pages → Source: *Deploy from a branch* → `main` → `/ (root)`.
3. Your site appears at `https://<user>.github.io/<repo>/`.

**Netlify**
1. Drag the whole folder onto app.netlify.com/drop, or connect the repo with no build command
   and publish directory `.`.

**Local preview** (optional): double-click `index.html`, or run
`python -m http.server 8000` in this folder and open `http://localhost:8000`.

## Updating the copy later

The authoritative text lives in `copy/sales-letter.md`. When the owner changes wording:

1. Edit `copy/sales-letter.md` first.
2. Mirror the change in `index.html`, keeping all styling and emphasis.
3. The page text must stay word-for-word identical to the copy file — including its
   existing typos and punctuation. Do not "fix" them.

If new text needs a character outside basic Latin (e.g. accented letters), the self-hosted
fonts will fall back for that character. The ₦ sign is already covered.

## Notes on the build

- Fonts are self-hosted; there are no Google Fonts requests.
- The ₦ sign uses a tiny dedicated font subset so the price renders on every Android phone.
- Orange is used **only** on the five buttons, nowhere else.
- The page works with JavaScript disabled; JS only adds UTM pass-through and the sticky
  mobile button.
- `utm_*` parameters in the page URL are copied onto the payment link automatically.
- No secrets, API keys, tracking pixels or third-party scripts are included.

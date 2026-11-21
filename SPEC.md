# **Build Specification: “Speak With Confidence” Sales Page**

**Purpose of this document:** a complete product requirements and build specification to give to OpenCode. Paste it in as the prompt, with the final copy file saved in the project as `copy/sales-letter.md` (the file is named `Sales-letter.md` on your side) before you start.

---

## **1\. Goal and success criteria**

Build a **single, fast, mobile-first sales landing page** that sells one product: an 8-week live English communication cohort at ₦150,000. The only conversion action is a click on a **“Reserve my seat”** button that opens an external Flutterwave payment link.

The page succeeds if:

1. A visitor on a mid-range Android phone, on mobile data, inside the TikTok in-app browser, can read the page comfortably and reach the payment link in one tap from anywhere on the page.  
2. The page looks credible and calm (education, trust), not hyped.  
3. The final copy appears **exactly** as supplied, with emphasis applied through styling only.  
4. Mobile Lighthouse scores: Performance 90+, Accessibility 95+, Best Practices 95+, SEO 90+.

## **2\. Audience and traffic context**

* Adults (working professionals, graduates, entrepreneurs, job seekers), mostly in Nigeria, some abroad.  
* Traffic comes from TikTok lives and videos (links in bio, comments, live). Expect 80%+ mobile, often Android, often slow or metered data, often inside TikTok’s in-app browser.  
* Visitors are sceptical. They are being asked for ₦150,000, so the page must feel professional, trustworthy and easy to read.

## **3\. Non-negotiable rules**

1. **Copy is final and verbatim.** Use the text in `copy/sales-letter.md` exactly. Do not rewrite, shorten, reorder, correct or “improve” any wording, punctuation or spelling, including apparent typos or inconsistencies (the owner will update the source file later). Keep curly quotes and apostrophes as they appear.  
2. **Emphasis is done with markup and CSS only** (bold, italic, size, colour, highlight, layout), as defined in Section 6\.  
3. **No invented content.** No fake testimonials, fake countdown timers, fake scarcity counters, “X people are viewing this”, stock claims or extra marketing text. The only text additions allowed are the placeholder tokens in Section 10, page metadata, and accessibility labels.  
4. **No frameworks and no build step.** Plain HTML, CSS and a small amount of vanilla JavaScript. No React, no Tailwind CDN, no jQuery, no external scripts.  
5. **No secret keys anywhere in the code.** The only payment-related value is a public Flutterwave payment link URL.  
6. **No pop-ups, modals, cookie banners, chat widgets or auto-playing media.** They behave badly in in-app browsers and distract from the single action.  
7. The instructor photo is a **placeholder for now** (Section 8).

## **4\. Tech stack and file structure**

```
/index.html            # all markup, inline <style> and a small inline <script>
/fonts/                # self-hosted woff2 files (Section 5.2)
/images/instructor-placeholder.svg
/images/og-placeholder.png   # 1200x630 placeholder for link previews
/copy/sales-letter.md        # source copy (authoritative)
/README.md             # how to replace tokens, swap the photo, deploy
```

* Inline the CSS in `index.html` (target under 20 KB unminified) so the first paint needs no extra CSS request.  
* Page weight target: HTML \+ CSS \+ fonts under 120 KB, excluding the photo. Photo (when added) under 150 KB, WebP.  
* Must work as a static site on any free host (Cloudflare Pages, GitHub Pages, Netlify).

## **5\. Design system**

**Design direction:** calm, credible, educational. Think trusted tutor or professional training institute, not flashy course-seller. Lots of white space, strong readability, one very obvious action colour.

### **5.1 Colour palette**

Research basis: blue is the most consistently associated colour with trust and security in finance and education branding. Research and A/B-test reviews show that no button hue wins universally; what lifts clicks is **contrast and visual isolation** (the button is the only element in its colour). So the page is built on navy and blue, and a single warm orange is reserved for the CTA and nothing else. All text pairs meet WCAG AA (4.5:1 or higher); verify with a contrast checker before shipping.

| Token | Hex | Use | Approx. contrast |
| ----- | ----- | ----- | ----- |
| `--navy` | `#0B2A4A` | Headings, strong text, footer background | 14.5:1 on white |
| `--text` | `#1F2937` | Body text | 14.5:1 on white |
| `--muted` | `#4B5563` | Secondary text, microcopy | 7.5:1 on white |
| `--blue` | `#1D5FA8` | Accent text, week labels, icons, borders, links | 6.4:1 on white |
| `--cta` | `#C2410C` | **Buttons only** (background) | white text 5.2:1 |
| `--cta-hover` | `#9A3412` | Button hover and active | white text 7:1+ |
| `--bg` | `#FFFFFF` | Main background | n/a |
| `--bg-tint` | `#F3F7FB` | Alternate section background | navy text 13:1 |
| `--green` | `#166534` | Guarantee panel icon and heading accent, “for you” checkmarks | 7:1 on white |
| `--green-tint` | `#ECF7F0` | Guarantee panel background | text 13:1 |
| `--highlight` | `#FFF3C4` | Marker highlight behind key phrases (max 4 uses on the whole page) | navy text 12:1 |
| `--border` | `#D6E0EB` | Card borders, dividers | n/a |
| `--x` | `#B42318` | “Not for you” icon colour only | icon only |

Rules:

* **Orange appears only on buttons.** Never use it for text, icons, borders or backgrounds anywhere else.  
* No gradients, no dark mode (set `color-scheme: light`), no decorative background images.  
* Section backgrounds alternate between `--bg` and `--bg-tint`. The only dark section is the footer.  
* Because buttons always sit on light backgrounds, no button appears on a navy surface.

### **5.2 Typography**

Research basis: serif headings signal authority and trust, while a clean sans-serif gives the best on-screen readability for long body text on phones. Pairing one serif with one sans is a standard, well-tested approach. Limit to two families and three weights to keep font weight light on slow connections. Body text should be at least 16px, line-height 1.5 to 1.8, and 45 to 75 characters per line.

| Role | Font | Weights | Notes |
| ----- | ----- | ----- | ----- |
| Headings (H1, H2, H3, pull-quotes) | **Lora** | **700** only | Warm, authoritative serif |
| Body, buttons, labels, FAQ, UI | **Inter** (variable) | **400** body, **500** minor emphasis, **600** labels and buttons, **700** bold (`strong`) | One variable file |

* **Self-host** the fonts as `woff2` (Latin subset), with `font-display: swap` and `<link rel="preload">` for the two main files. Total font payload target: under 70 KB.  
* **Naira sign (₦, U+20A6):** it is not in every Latin subset. Include the `latin-ext` range for Inter (or add `U+20A6` to the `unicode-range`) and set the price in Inter, not Lora. Verify ₦ renders correctly on a real Android phone.  
* Fallback stacks:  
  * Lora: `'Lora', Georgia, 'Times New Roman', serif`  
  * Inter: `'Inter', system-ui, -apple-system, 'Segoe UI', Roboto, Arial, sans-serif`

Type scale (mobile first, then desktop at 768px and up):

| Element | Mobile | Desktop | Line height |
| ----- | ----- | ----- | ----- |
| H1 (page headline) | 30px | 44px | 1.2 |
| H2 (section headings, copy is uppercase) | 22px, letter-spacing 0.02em | 28px | 1.25 |
| H3 (week titles) | 19px | 21px | 1.3 |
| Body | **18px** | 19px | 1.65 |
| Subheadline (hero) | 19px | 21px | 1.55 |
| Pull-quote | 22px | 26px | 1.4 |
| Microcopy, labels | 15px | 15px | 1.5 |
| Button label | 18px (Inter 700\) | 18px | 1 |
| Price | 44px (Inter 700\) | 56px | 1.1 |

Text measure: body copy blocks max-width `66ch` (about 600 to 680px). Never run full-width text across a desktop screen.

### **5.3 Spacing, shape, depth**

* 8px spacing scale. Section vertical padding: 56px mobile, 96px desktop. Paragraph spacing: 20px.  
* Content container: max-width 720px for text sections; 1040px for the hero and two-column layouts; 20px side padding on mobile.  
* Border radius: 12px cards, 10px buttons, 999px for chips.  
* Shadows: only a soft card shadow `0 2px 12px rgba(11,42,74,0.08)` and the button shadow below.  
* Icons: simple inline SVG (check, X, shield, calendar, arrow), 20 to 24px, `aria-hidden`, no icon fonts or icon libraries.

### **5.4 Core components**

**Primary button (`.btn-cta`)**

* Background `--cta`, text white, Inter 700 18px, label exactly **Reserve my seat** (sentence case, no uppercase transform), a small right-arrow icon after the label.  
* Minimum height 56px, padding 0 28px, radius 10px. Full width on mobile, auto width (min 320px) on desktop.  
* Shadow `0 6px 16px rgba(194,65,12,0.28)`. Hover and active: background `--cta-hover`, move up 1px. Visible focus ring: 3px `--blue` with 3px offset.  
* Same-tab link (no `target=_blank`).

**Marker highlight (`mark`)**: `--highlight` background on the lower 60% of the line (linear-gradient trick), navy text, no extra padding.

**Pull-quote callout**: 4px `--blue` left border, 20px left padding, Lora 700, navy.

**Card**: white (or tint) background, 1px `--border`, 12px radius, 24px padding mobile and 32px desktop.

**Checklist**: blue check icon in a 24px circle, 12px gap, 16px between items.

**FAQ accordion**: native `details` and `summary`, no JavaScript. Summary text Inter 600, navy, with a plus or chevron that rotates when open. 56px minimum tap height, 1px bottom borders.

## **6\. Page structure and emphasis rules**

Build the sections in exactly this order. Quote each heading and paragraph from the copy file. In the copy, a single `#` is a section heading (use `h2`), `##` is a week heading (use `h3`), and the first line of the file is the page headline (`h1`). `{Reserve my seat}` means a **primary button**.

### **6.1 Hero (white background)**

* Content: the headline, the supporting paragraph beneath it, a **Reserve my seat** button, and the line beginning “Limited to 20 participants…” as microcopy under the button (15px, `--muted`, italic, with a small people icon).  
* Layout: desktop two columns (text about 60%, instructor photo placeholder about 40%). Mobile single column: text first, button, microcopy, then the photo placeholder below.  
* **Headline emphasis:** the headline is Lora 700 navy; style the final phrase **“Speaking With Clarity and Authority.”** in `--blue`.  
* **Supporting paragraph emphasis:** bold **“over 5,000 students in 10 years”** and **“real speaking practice”**.

### **6.2 The letter: “Dear friend,” through “…designed to help you do.” (white)**

A personal, letter-style block in a single 640px column.

* “Dear friend,” in Inter 600, 20px.  
* “Picture this;” in italics.  
* The short lines (“You are in a meeting.” through “You have actually known it for a while.”) are a **staccato stack**: each on its own line, 22px, Lora 700 navy, 12px spacing, inside a block with a `--blue` left border. These lines are the visual hook and should be larger than body text.  
* In “But before you open your mouth…”, set the quoted question fragments in *italic*: “I have been”, “I was”, and the two sentences that follow them.  
* “The moment is gone.” on its own line, **bold**.  
* “Next time.” inside the quote marks in *italic*.  
* “If that is you, I want you to hear this first: it is not because you are unintelligent.” is the **pull-quote callout**, with **“it is not because you are unintelligent.”** wrapped in `mark`.  
* Bold **“What they lack is confidence”** in its sentence.  
* Bold **“It is a skill”** in the “Confidence in English…” sentence.  
* “That is exactly what this 8-week cohort is designed to help you do.” is Lora 700, 22px, navy, set apart with extra space above and below.

### **6.3 Why nothing worked: “You have probably tried to improve.” through “…structured and practical approach.” (tint)**

* Normal paragraphs, with “Here is why:” in **bold**.  
* The three reasons are **three cards**, each with a decorative number badge (1, 2, 3\) made with a CSS counter. In each card, the **first sentence is bold** (“You learned grammar to pass exams, not necessarily to communicate.”, “You may not have had a safe place to practise out loud.”, “You may never have followed a structured system.”).  
* The final two sentences (“The problem is not that you cannot improve…”) are a pull-quote callout.

### **6.4 “INTRODUCING: THE SPEAK WITH CONFIDENCE COHORT” (white)**

* `h2`, then the paragraph. In the paragraph, bold **“live, 8-week small-group virtual programme”**.  
* “What you will learn;” as a bold label.  
* **Eight week items** shown as a vertical timeline (thin `--border` line on the left, blue dot per week), all expanded (no accordion).  
  * Render each week heading as a small blue label chip (“WEEK 1:” in Inter 600, 13px, tracking 0.08em) followed by the rest of the heading as the `h3` in Lora 700\. The text stays exactly as in the copy; only the split into two spans is styling.  
  * The lines beneath each heading are normal paragraphs, `--text`, 18px.  
* For Week 8, bold **“speaking showcase”**.

### **6.5 “EVERYTHING YOU GET” (tint)**

* `h2`, then the seven items as a **checklist** (Section 5.4).  
* Bold the lead phrase in each item: **“Eight weeks of live classes on Google Meet”**, **“A small, supportive group”**, **“Weekly speaking practice”**, **“Class recordings”**, **“A private WhatsApp community”**, **“A starting and ending speaking assessment”**, **“Practical workbooks and learning resources”**.  
* Add a **Reserve my seat** button under the list. This is an extra button placement using the same label.

### **6.6 “WHO THIS IS FOR” and “WHO THIS IS NOT FOR” (white)**

* Two cards side by side on desktop, stacked on mobile. “For” card has a 4px `--green` top border and green check icons. “Not for” card has a 4px `--border` top border and `--x` X icons, in a quieter tone.  
* “This cohort is for you if:” in bold.  
* In the “not for” list, bold the first clause of each item (**“You want instant fluency without putting in the practice.”**, **“You are unwilling to participate in speaking activities, even in a small group.”**, **“You are looking for examination preparation such as WAEC or JAMB.”**).

### **6.7 “WHY LEARN FROM ME?” (tint)**

* Two columns on desktop: instructor photo placeholder on the left (about 35%), copy on the right. Mobile: photo placeholder first, then copy. The placeholder should be a rounded rectangle (4:5 ratio).  
* Bold the proof points inline, with no new stat strip or extra text: **“10 years”**, **“more than 5,000 students”**, **“over 5 million times”**, **“OAU, UI, FUTA and UNILORIN”**, **“the USA, the UK and Canada”**.

### **6.8 “WHAT OTHERS ARE SAYING” (white)**

* The copy file contains bracketed instructions to the owner here. **Do not render those bracketed notes on the page.**  
* Build the section with three testimonial card placeholders (avatar circle, quote area, name line) and mark the whole section with the `hidden` attribute so it is **not visible** until real testimonials are added. Document in the README how to unhide it.  
* Style, for when it goes live: quote in Inter 400 italic 18px, name in Inter 600, card layout as in Section 5.4.

### **6.9 “YOUR INVESTMENT” (white, content inside a centred card)**

* `h2`, then the first paragraph.  
* “Your investment for the full 8-week cohort:” on one line as a label, then **₦150,000** on its own line as the **price** (Inter 700, 44px mobile, 56px desktop, navy).  
* Then “Your payment covers…” as normal text.  
* “Think about it this way:” in **bold**, with the sentence that follows and the next two short lines as normal text in a single flow.  
* **Reserve my seat** button at the bottom of the card.

### **6.10 “MY PROMISE TO YOU” (green-tint panel)**

* `h2` with a shield icon in `--green`, set in a rounded panel on `--green-tint` with a 1px `--green` border at low opacity.  
* The two guarantee paragraphs, normal text. Bold **“Join the first two sessions”**.  
* The next paragraphs (“We are only taking…” and “Once all 20 seats are filled, registration will close.”) follow as normal text.  
* The line **“Registration closes on the 2nd of December. Classes begin on the 4th of December.”** is a **date callout**: bold navy, calendar icon, white card inside the panel, with `mark` on “2nd of December” and “4th of December”. No countdown timer.

### **6.11 “HOW IT WORKS” (white)**

* `h2`, the two paragraphs as normal text, then a **Reserve my seat** button.

### **6.12 “FREQUENTLY ASKED QUESTIONS” (tint)**

* `h2`, then each question and answer as an accordion item. The question text is exactly as in the copy, including the curly quotes. All items closed by default. Answers are normal text.

### **6.13 Footer (navy background, white text at 80% opacity)**

* Only placeholder tokens: `© 2026 [TUTOR NAME]` (Inter 400, 14px, centred). No navigation, no social icons, no extra links.

### **6.14 Sticky mobile button**

* On screens under 768px, show a fixed bottom bar containing one **Reserve my seat** button once the user has scrolled past the hero button.  
* Hide the bar while any other **Reserve my seat** button is visible in the viewport (use `IntersectionObserver`) so two identical buttons never show at once.  
* Bar: white background, 1px top border `--border`, padding 12px 16px plus `env(safe-area-inset-bottom)`. Add bottom padding to the page so the bar never covers content.

## **7\. CTA behaviour**

* Every **Reserve my seat** button is an `<a>` styled as a button, with `href` set to the token `__FLUTTERWAVE_PAYMENT_LINK__` in one consistent format so the owner can find and replace it everywhere in one step.  
* Buttons: hero, after “Everything you get”, in “Your investment”, in “How it works”, and the sticky bar.  
* Give each button a `data-cta` attribute naming its position (`hero`, `features`, `investment`, `how-it-works`, `sticky`) so click tracking can be added later.  
* Include a small, commented JavaScript block that passes through any `utm_*` query parameters from the page URL onto each CTA link. It must fail silently, and the buttons must still work with JavaScript disabled.  
* Include a clearly marked comment block in `<head>` where an analytics snippet can be pasted.

## **8\. Instructor photo placeholder**

* Use `images/instructor-placeholder.svg`: a 4:5 rounded rectangle in `--bg-tint` with `--border` outline and a neutral person silhouette in `--blue` at low opacity. No text inside it.  
* Use it in two places: the hero and “Why learn from me”.  
* Set explicit `width` and `height` attributes to prevent layout shift, `loading="lazy"` on the lower image, and meaningful alt text: `[TUTOR NAME], English language tutor`.  
* README must explain how to replace it with a real photo (WebP, under 150 KB, 4:5 ratio, 800 x 1000 px, face centred in the upper half).

## **9\. Responsive, performance and in-app browser requirements**

* Mobile first. Breakpoints at 768px and 1100px. No horizontal scrolling at any width from 320px up.  
* Tap targets at least 48px. No hover-only interactions.  
* No external requests except the payment link. No Google Fonts requests, no third-party scripts.  
* Use `100dvh` (with a fallback) rather than `100vh` for any full-height element, because address bars in in-app browsers resize unpredictably.  
* No `position: fixed` elements other than the sticky bar.  
* Respect `prefers-reduced-motion`. Animations are limited to subtle button hover and accordion rotation.  
* Do not use JavaScript for anything the page needs to be readable or for the buttons to work.

## **10\. Placeholder tokens (owner replaces later)**

| Token | Where |
| ----- | ----- |
| `__FLUTTERWAVE_PAYMENT_LINK__` | Every CTA `href` |
| `[TUTOR NAME]` | Footer, photo alt text |
| `images/instructor-placeholder.svg` | Hero and about section |
| `images/og-placeholder.png` | Open Graph and Twitter image |
| Testimonials section | Hidden until real testimonials are added |
| Analytics snippet | Commented block in `<head>` |

## **11\. SEO, metadata and sharing**

* `<html lang="en-NG">`, UTF-8, viewport meta (`width=device-width, initial-scale=1`), `theme-color` `#0B2A4A`.  
* `<title>`: “Speak With Confidence: 8-Week Live English Cohort”.  
* Meta description: use the page’s supporting paragraph (first paragraph under the headline) trimmed to about 155 characters, without changing its wording.  
* Open Graph and Twitter Card tags (title, description, `og:image` placeholder, `summary_large_image`) so the link previews properly when shared on WhatsApp and TikTok.  
* Inline SVG favicon or `favicon.svg`. Semantic HTML throughout (`header`, `main`, `section`, `footer`), one `h1`, a logical `h2` and `h3` order.

## **12\. Accessibility**

* All text meets WCAG AA contrast (Section 5.1).  
* Visible keyboard focus on every interactive element.  
* Decorative SVGs are `aria-hidden="true"`. Buttons are real links with descriptive text.  
* Accordion uses native `details`/`summary`.  
* The page remains fully readable at 200% zoom.

## **13\. Acceptance checklist (run before finishing)**

1. Extract the visible text from `index.html` and diff it against `copy/sales-letter.md`. Report any difference. Differences must be zero, apart from the footer, the hidden testimonials section and the omitted bracketed notes.  
2. Confirm every emphasis rule in Section 6 is applied, and that `mark` is used no more than four times in total.  
3. Confirm orange (`--cta`, `--cta-hover`) is used for buttons only.  
4. Confirm the five CTA placements exist, each with the correct `data-cta` value, and that the sticky bar never overlaps another visible button.  
5. Confirm the fonts load locally, `₦150,000` renders correctly, and the page looks acceptable with fonts blocked (fallback stacks).  
6. Test at 320px, 375px, 414px, 768px and 1280px widths. No horizontal scroll, no overlapping text.  
7. Run Lighthouse in mobile mode and report scores (targets in Section 1).  
8. Check total payload: HTML plus CSS plus fonts under 120 KB.  
9. Search the code to confirm there are no API keys, secrets, external scripts or tracking pixels.

## **14\. Deliverables**

* The files listed in Section 4, working locally when `index.html` is opened or served from a static host.  
* A short `README.md` covering: how to replace the payment link, tutor name and photo; how to unhide the testimonials; how to add analytics; how to deploy to Cloudflare Pages, GitHub Pages or Netlify; and how to update copy later.  
* A brief final report listing anything that could not be implemented as specified and any decisions you made.

## **15\. Out of scope**

No checkout built into the page, no backend, no form collection, no email capture, no blog, no multi-page navigation, no countdown timer, no popups, no animation libraries, no dark mode.


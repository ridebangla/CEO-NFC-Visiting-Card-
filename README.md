# Ride Bangla — Digital Business Card

A single-file, self-contained digital business card for **Enamul Seddik**, Co-Founder & CEO of Ride Bangla. Built as one `index.html` (no build step, no framework) so it can be deployed anywhere as a static site — Vercel, Netlify, GitHub Pages, or any web host.

|                      Front                       |                       Back                       |
| :-----------------------------------------------: | :-----------------------------------------------: |
| ![Logo](logo.png)                                 | ![Photo](photo.png)                                |

*(the images above are the actual processed assets used on the card — see "Assets" below)*

---

## What's on the card

**Front**
- Ride Bangla logo, name, title, headshot
- Phone / email / website / location — all tap-to-act links
- "Tap to Connect" hint (NFC-style) — tapping the card flips it

**Back** (revealed by flipping the card)
- Logo, QR code (auto-generated from the card's own live URL), "Scan & Connect" script tag
- Social/contact tiles: Website, WhatsApp, Facebook, Location (LinkedIn/YouTube tiles auto-appear once you add their links)
- "About Us" blurb, fully contained inside the card border

**Below the card**
- Save contact (downloads a `.vcf`), Call, Email quick-action buttons
- A "Scan & Connect ⇄ Back to card" button that flips the card the same way tapping it does
- An "Our Services" grid and footer

---

## How the flip works

The old version used a drawer that expanded *below* the card. This version does a real **3D card flip**, matching the physical card mockup:

- `.flipcard` is the sizing box (fixed 5:8 aspect ratio, so it always looks like a real ID/business card).
- `.flipcard-inner` holds both faces and rotates 180° on the Y-axis when the `.flipped` class is toggled.
- Both `.card.front` and `.card.back` are absolutely stacked on top of each other with `backface-visibility: hidden`, so only one is ever visible.
- **Tap anywhere on the front face** → flips to the back.
- **"Back" arrow (top-right of the back face)** or the **bottom "Back to card" button** → flips back to the front. (The back face is full of real links, so only a small dedicated control flips it back — this stops an accidental tap on a tile from also flipping the card.)
- Everything is keyboard accessible (front face is a focusable `role="button"`, `Enter`/`Space` flips it).

## Matching the physical card mockup

The card's decorative details now follow the actual mockup design (not the earlier placeholder look):
- The bottom edge no longer has a full-width wave — it's a diagonal green/red ribbon confined to the **bottom-right corner**, with thin gold pinstripes, on both faces.
- The QR code sits inside a **corner-bracket frame** (green brackets on the left, red on the right) with a curved gold arrow linking it to the "Scan & Connect" script text — matching the mockup's scan callout.
- The "About Us" block is now plain (no separate shaded box) with just a thin gold hairline above it, so it reads as one continuous card surface like the mockup.

## Layout fix (About Us overflow)

The previous "About Us" block used a full-bleed width trick (`width: calc(100% + 32px)`) without a matching negative margin, so it rendered wider than the card and spilled past the gold border. It's now:
- centered correctly with `margin: 0 -16px` matching its own padding, so the bleed is intentional and even on both sides,
- sized and paletted to always sit fully inside the card's rounded corners, at every card width down to ~320px (tested).

## Assets — how the logo & photo were prepared

- **`logo.png`** — the original logo had a flat white background. It's been background-removed (the outer white area only — the white Bangladesh-map cutout *inside* the scooter icon was preserved) and tightly cropped, so it now sits on the dark green card with a clean edge and no white box around it. A subtle white drop-shadow is used in CSS to keep it legible against the dark background.
- **`photo.png`** — the headshot was cropped to a clean square centered on the face/shoulders, so it fills the circular frame on the card perfectly with no awkward cropping.

If you ever swap either image, just replace the file with the same name (`logo.png` / `photo.png`) — no HTML changes needed. For best results, keep the logo on a transparent background and the photo roughly square.

## Files

```
index.html    — the entire card (structure, styles, and logic in one file)
logo.png      — background-removed Ride Bangla logo
photo.png     — cropped headshot
vercel.json   — static hosting headers/config for Vercel
README.md     — this file
```

## Customize

Open `index.html` and edit the `LINKS` object near the bottom of the `<script>` — set a URL to show that tile, leave it as `""` to hide it:

```js
const LINKS = {
  website:  "https://ridebangla.bd",
  whatsapp: "https://wa.me/message/...",
  facebook: "https://www.facebook.com/...",
  linkedin: "",   // add to show the tile
  youtube:  "",   // add to show the tile
  location: "https://www.google.com/maps/..."
};
```

Phone, email, and the "Save contact" vCard fields are set directly in the HTML/JS near the top of `<body>` and in the `$('save').onclick` handler.

## Deploy

This is a static site — drop the folder into Vercel (the included `vercel.json` is already set up) or any static host. No build step required.

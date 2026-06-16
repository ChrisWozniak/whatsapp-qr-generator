# WhatsApp Contact QR Generator

A browser-based tool that generates scannable WhatsApp QR codes from a name and phone number — no app, no sign-up, no backend required.

Built by **Chris Wozniak** and **Nathan Hutton** as a two-stage group project.

---

## What it does

Enter your name and phone number, click Generate, and the app produces a QR code that encodes a `wa.me/` deep link. Anyone who scans it with their phone camera is taken directly into a WhatsApp conversation with you — no manual number entry required.

The generated code can be saved as a branded PNG card (name, avatar, and QR framed together) suitable for business cards, social profiles, or anywhere else you want to share your WhatsApp contact.

---

## Why we built it

WhatsApp's own profile QR feature is buried inside the app and can't be downloaded in a clean, shareable format. There's no native way to generate a contact QR from a desktop browser or embed one into a flyer. This tool fills that gap: it runs entirely in the browser, requires no account, and produces a ready-to-share image in two clicks.

The project also doubles as a learning exercise in freemium product design — the free tier allows 3 generations, after which users see an upgrade screen for a planned Premium tier.

---

## Stage 1 — Completed (Cycle 1, Week 1)

Everything in the current codebase represents Stage 1:

- **WhatsApp-style UI** — dark theme matching WhatsApp's exact color palette (`#111b21`, `#25D366`, `#1f2c34`)
- **Live name preview** — the topbar updates in real time as the user types
- **QR generation** — encodes a `wa.me/<number>` deep link; auto-prefixes country code `+1` for 10-digit numbers
- **WhatsApp logo overlay** — a phone icon is drawn into the center of every QR code using the Canvas API
- **Branded PNG export** — downloads a polished card image (480 px wide) with avatar, name, subtitle, framed QR, and footer
- **Freemium counter** — tracks up to 3 free generations via `localStorage`; disables the button and shows an upgrade screen at the limit
- **Upgrade screen** — lists Premium features and shows a placeholder CTA (active in Stage 2)
- **Responsive layout** — mobile-first, adapts to desktop at 520 px breakpoint
- **Presentation reset** — `Ctrl+\` on desktop or **5 taps on the "WhatsApp contact" subtitle** on mobile resets the freemium counter; both show a brief "Counter reset!" confirmation in the topbar

---

## Stage 2 — In progress (Cycle 1, Week 2)

Based on the *Week 2 Improvements* PRD (Goal-Gradient Progress Bar + Premium Feature Unlock).

### Implemented

- **Goal-Gradient progress bar** — the text-only freemium counter is replaced with a visual progress bar that applies the Goal-Gradient Effect (Laws of UX). The track is gray before any use, then turns WhatsApp green (`#25D366`) once generation starts. An amber (`#FFA500`) fill grows left-to-right across the green as the user approaches the gate — **1/3 amber after the 1st code, 2/3 after the 2nd, fully amber after the 3rd**. The amber width animates smoothly on each generation.
- **Dynamic counter copy** — the label below the bar updates with the remaining count: "3 free codes remaining" → "2 free codes remaining" → "Last free code — make it count!" → "You've used all 3 free codes".
- **Realistic iPhone device frame (desktop)** — on screens ≥ 520 px the app is rendered inside a true-to-life iPhone 17 frame: black anodized casing, Dynamic Island, iOS status bar (9:41, signal, Wi-Fi, battery), and physical side buttons (mute, volume, power). The frame uses authentic **iPhone 17 proportions (~19.5:9)** and scales fluidly to the viewport — capped at 902 px tall on large monitors and shrinking on smaller screens while preserving its shape — and stays centered. On mobile the frame is hidden and the app fills the screen as a normal full-screen web app.

### Still planned

- **Payment / Premium unlock** — real upgrade flow replacing the placeholder alert
- **Unlimited generations** — no cap for Premium users
- **Custom brand colors** — user-selectable QR foreground and background colors
- **Custom logo upload** — replace the default WhatsApp overlay with the user's own logo
- **SVG / PDF export** — additional download formats for print-quality output
- **Live QR preview** — preview that updates as the user types
- **Persistent Premium state** — remember paid status across sessions

---

## Technology stack

| Layer | Technology |
|---|---|
| Markup | HTML5 |
| Styling | CSS3 (custom, no framework) |
| Logic | Vanilla JavaScript (ES6+) |
| QR encoding | [QRCodeJS 1.0.0](https://github.com/davidshimjs/qrcodejs) via CDN |
| QR rendering & export | Canvas API + Path2D |
| Fonts | Google Fonts — Inter (400, 500, 600) |
| State persistence | `localStorage` (freemium counter) |
| Hosting | Static file — no server required |

No build step, no bundler, no framework. Open `index.html` in a browser and it works.

---

## Project structure

```
whatsapp-qr-generator/
├── index.html      # Entire app — markup, styles (inline), and scripts
├── style.css       # Placeholder (styles live in index.html)
├── app.js          # Placeholder (scripts live in index.html)
├── pic1.PNG        # Reference / design asset
├── pic2.PNG        # Reference / design asset
└── README.md
```

---

## Secure Build Checklist — Cycle 1, Week 1

Security is treated as a first-class requirement, not an afterthought. The following four checks from the project's Secure Build Checklist guide every release.

### Check 1 — Validate all user inputs
**Status: Fully implemented**

Both fields carry an HTML `maxlength` attribute as a first gate (name: 100 chars, phone: 20 chars). On submission, `validatePhone()` counts the digits extracted from the phone field and rejects anything outside the 7–15 digit E.164 range with an inline error message rendered in the form. The Generate button remains disabled until both fields are non-empty, and `sanitizeName()` is run on the name before it is used anywhere in the app. Invalid input never reaches the QR generation step.

### Check 2 — Sanitize inputs to prevent injection
**Status: Fully implemented**

- **Name field** — `sanitizeName()` strips all control characters (`\x00–\x1F`, `\x7F`) and caps the value at 100 characters before it is used anywhere.
- **Phone field** — reduced to digits only by `buildWaMe()` before being embedded in the `wa.me/` URL; no other characters can reach the payload.
- **DOM output** — the name is written exclusively via `.textContent` and Canvas `fillText`, never `innerHTML`, so HTML or script tags are always rendered as literal text.
- **Download filename** — the filename is passed through an additional filter that removes any characters outside `[a-zA-Z0-9 _-]`, preventing path traversal sequences (e.g. `../../evil`) from appearing in the saved file name.

### Check 3 — No API keys or secrets in the codebase
**Status: Fully implemented**

The app is entirely client-side and calls no external APIs that require authentication. There are no tokens, passwords, or secrets in any file. The only external requests are the QRCodeJS library and Inter font loaded from public CDNs. Verified clean before every push.

### Check 4 — Validate QR code destination is safe
**Status: Fully implemented**

`buildWaMe()` was audited and a passthrough vulnerability was found and removed: the original code returned a user-supplied `https://wa.me/` string verbatim, meaning a user could embed arbitrary query parameters (e.g. `?text=phishing message`) directly into the QR payload. The function now always extracts digits from the raw input and constructs the URL from scratch — every QR code can only ever encode a clean `https://wa.me/<number>` link with no query string. Arbitrary destination URLs cannot be injected through either input field.

---

## Running the project

No installation needed. Clone or download the repo and open `index.html` in any modern browser.

```
git clone https://github.com/ChrisWozniak/whatsapp-qr-generator.git
cd whatsapp-qr-generator
# open index.html in your browser
```

The app works fully offline after the initial CDN load (QRCodeJS + Inter font).

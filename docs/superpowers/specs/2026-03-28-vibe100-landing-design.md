# Vibe100 Landing Page — Design Spec

**Date:** 2026-03-28
**Status:** Approved

---

## Overview

Single-page landing site for **Vibe100** — a 100-day daily AI building challenge. The goal is to collect email signups and feed them into a Kit sequence. No framework, no build step — pure HTML/CSS/JS deployed to Cloudflare Pages at `vibe100.co`.

---

## Architecture

- **Stack:** Single `index.html` file — HTML, CSS, JS inline
- **Hosting:** Cloudflare Pages, custom domain `vibe100.co`
- **Email:** Kit form ID `9259428` — POST to `https://app.convertkit.com/forms/9259428/subscriptions`
- **No dependencies** — Google Fonts loaded via `@import`, no npm, no build step

---

## Visual Design

| Token | Value |
|-------|-------|
| Background | `#0a0a0a` |
| Accent / orange | `#C4773A` |
| Body text | `#777` |
| Dim text | `#444–#555` |
| Bright text | `#aaa–#bbb` |
| White | `#ffffff` |

**Fonts:**
- `Syne 800` — hero title, CTA button
- `IBM Plex Mono` — all body copy, labels, terminal text, input, fine print

**Effects:**
- Scanline texture overlay (repeating CSS gradient, 4px pitch, 6% opacity)
- Glitch animation on hero title: periodic RGB split (red + cyan layers, `clip-path` slices), triggers every ~4s
- Thin blinking cursor (`border-right`) on typewriter row

---

## Page Structure (top to bottom)

1. **Terminal bar** — decorative macOS-style dots + `vibe100.co — session active` label
2. **Badge** — `// 100-day AI building challenge` in orange, monospaced, pill border
3. **Hero title** — `vibe` (white) + `100` (orange), Syne 800, ~80px, same size, glitch animation
4. **Subtext** — IBM Plex Mono 13px: `"A daily building challenge. 100 prompts, 100 builds, shipped publicly."`
5. **Typewriter row** — `›` arrow + cycling text + thin blinking cursor. Cycles through:
   - `The best way to go from AI-curious to AI-native.`
   - `Join and receive a daily prompt every day.`
   - `Build with any tool you like.`
   - `Share your build and get featured.`
6. **Email form** — input placeholder: `Enter your email to receive daily prompts` + button: `Join Vibe100`
7. **Fine print** — `No spam. No data sharing. Just build prompts.`
8. **Success state** — replaces form on submit: `You're in. Day 1 prompt is on its way. 🚀`
9. **Stats row** — `100 days / 1/day / ∞ shipped`, separated by top border

---

## Kit Integration

```
POST https://app.convertkit.com/forms/9259428/subscriptions
Content-Type: application/json

{ "email_address": "<user input>" }
```

- On success (2xx): hide form, show success message
- On error: show inline error in IBM Plex Mono below the input

---

## Typewriter Behavior

- Types each sentence character by character at 45ms/char
- Pauses 1800ms at full length
- Deletes at 25ms/char
- Pauses 300ms before next sentence
- Loops infinitely

---

## Animations

| Element | Animation |
|---------|-----------|
| Hero title | Glitch burst every 4s — RGB split via `::before`/`::after` + `clip-path` |
| Typewriter cursor | `border-right` blink, 1s step-end |
| Scanlines | Static CSS overlay, no animation |

---

## Error Handling

- Empty/invalid email: native browser `type="email"` validation before submit
- Kit API error: inline error message below the input, IBM Plex Mono, dim red `#ff6b6b`
- Network failure: same inline error treatment

---

## Deployment

1. Push `index.html` to GitHub repo
2. Connect repo to Cloudflare Pages
3. Set custom domain `vibe100.co` in Cloudflare Pages settings
4. Add `.superpowers/` to `.gitignore`

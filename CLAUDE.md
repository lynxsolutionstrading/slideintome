# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

SlideInto.me is a Telegram-based anonymous social content discovery platform. Users authenticate via Telegram (no email/password), then browse Instagram, Twitter, and TikTok content feeds and save items to a personal list.

## Development

No build system — the entire app is a single self-contained file: `index.html` (all CSS in `<style>`, all JS in `<script>`).

To serve locally:
```bash
python -m http.server 8000
# or
npx http-server
```

There are no tests, no linter configs, and no npm dependencies.

## Architecture

Everything lives in `index.html`. The UI has two main states:

1. **Login screen** — shown on load. Contains:
   - Mobile layout (393×852px mockup): logo → login form
   - Desktop layout (≥768px): left hero panel + right login panel with divider
   - Telegram login button + creator code input

2. **Logged-in screen** (`id="loggedInScreen"`) — revealed after login via fade overlay (`id="fadeOverlay"`). Contains:
   - Top bar with Topup button
   - Three tab buttons: Instagram, Twitter, TikTok
   - Dynamic image grid (`id="imageGrid"`)

### Key JavaScript Functions

| Function | Purpose |
|---|---|
| `buildGrid(tab)` | Generates 18-item grid HTML using gradient backgrounds from `tabTiles` |
| `switchTab(newTab)` | Slide animation between tabs; direction determined by `tabOrder` index diff |
| `toggleAddList(btn)` | Toggles "Add to my list" / "Remove" state on a grid item |
| `handleLogin(btn)` | 2-second simulated login → fade overlay → show logged-in screen + toast |
| `applyLayout()` | Switches between mobile/desktop DOM layout on window resize |

### Data

- `tabTiles` — object mapping `instagram`/`twitter`/`tiktok` to arrays of CSS gradient strings used as placeholder backgrounds
- `tabOrder` — `['instagram', 'twitter', 'tiktok']` defines tab sequence for slide direction logic

### Design System

- Dark theme: `#000000` background, `#ffffff` text, `#c0392b` red accent (CTAs)
- Font: Google Fonts "Inter" (400/500/600/700), loaded via CDN
- Mobile-first responsive at `768px` breakpoint
- Animations: tab slides use `cubic-bezier(0.42, 0, 0.18, 1.0)`, logo uses 2s fade-in

### Login Flow (currently simulated)

`handleLogin()` fakes authentication with a 2s timeout. Real Telegram Bot auth would replace this — the Telegram Login Widget or bot deep-link flow needs to be wired in, along with creator code validation on a backend API.

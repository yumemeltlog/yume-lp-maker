# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

**ゆめLPメーカー** (Yume LP Maker) is a single-page Japanese tool that generates two types of AI prompts from product/service information entered via a web form:

1. A **Claude prompt** — instructs Claude to build a complete responsive HTML landing page (LP)
2. A **Midjourney prompt** — generates LP hero visuals and section assets in the chosen aesthetic

The entire application is one file: `index.html`. There is no build step, no package manager, and no external JavaScript dependencies.

## Running the app

Open `index.html` directly in a browser — no server needed. For development, any static file server works:

```bash
python3 -m http.server 8080
# or
npx serve .
```

## Architecture

Everything lives in `index.html` as three sections:

- **`<style>`** — all CSS, using CSS custom properties defined in `:root` for the dark pink/cherry color palette (`--pink`, `--dark`, `--card`, etc.)
- **`<body>`** — semantic HTML with a `<header>`, `<main>` (form + output), and `<footer>`
- **`<script>`** — three vanilla JS functions:
  - `generatePrompts()` — reads form values, assembles the Claude and Midjourney prompt strings via template literals, writes them to `#claudePrompt` / `#mjPrompt`, and scrolls output into view
  - `switchTab(tab)` — toggles `.active` on `.tab-btn` and `.tab-pane` elements
  - `copyText(id, btn)` — copies `.innerText` of the target element via `navigator.clipboard.writeText()`

## Key design decisions

- **Vibe checkboxes** use a `.checked` CSS class toggled via a `click` listener with `setTimeout(..., 0)` to let the checkbox's native checked state update before reading it
- The **Midjourney prompt** includes a `vibeMap` object that translates each Japanese vibe option into English Midjourney keyword phrases
- Only `商品・サービス名` (product name) and `価格` (price) are required; all other fields fall back to `'記載なし'` or `'おまかせ'` in the generated prompt
- The page is fully self-contained; Google Fonts (`Noto Sans JP` + `Fredoka One`) is the only external dependency (loaded via CDN)

## Conventions

- Japanese UI strings: labels, placeholders, and error messages (`alert(...)`) are in Japanese
- Midjourney prompt keywords remain in English (intentional — Midjourney performs better with English prompts)
- No CSS preprocessor; media queries for mobile breakpoints are at the bottom of `<style>` (`@media(max-width:640px)`)

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

This repo is a small collection of standalone, static HTML files, each a self-contained embeddable player meant to be hosted and loaded via `<iframe>`. There is no build system, package manager, bundler, server code, or test suite — every file is plain HTML/CSS/JS that runs directly in a browser, with all dependencies pulled from public CDNs at runtime.

## Files

- `index.html` — the HLS video player (referred to as `iframe-player.html` in the README). Renders a Video.js player, uses `hls.js` for HLS (`.m3u8`) playback, and optionally loads a WebVTT subtitle track. Reads its configuration entirely from URL query parameters:
  - `video` — URL of the HLS video source (required; spaces are decoded from `+`)
  - `subtitle` — URL of a WebVTT subtitle file (optional; same `+`-to-space decoding)
- `anime-player.html` — a generic iframe wrapper. Reads a `link` query parameter (URL-encoded), and sets it as the `src` of a full-viewport inner `<iframe>`. Used to embed arbitrary third-party players/pages rather than playing HLS directly.
- `README.md` — user-facing documentation for `index.html` (documented there as `iframe-player.html`): embedding examples, query parameter table, and hosting instructions (GitHub Pages, Vercel, Netlify, or `npx http-server` locally).

## Development workflow

There is no build, lint, or test tooling in this repo — do not introduce a package.json, bundler, or test framework unless explicitly asked. To work on a player:

1. Edit the relevant `.html` file directly (all markup, styles, and JS live inline in the file).
2. Serve the repo root locally to test in a browser, e.g. `npx http-server`.
3. Exercise the player through its query-parameter interface, e.g.:
   `http://localhost:8080/index.html?video=https://test-streams.mux.dev/x36xhzz/x36xhzz.m3u8&subtitle=https://example.com/subtitles.vtt`
   `http://localhost:8080/anime-player.html?link=https%3A%2F%2Fexample.com%2Fembed`

## Conventions

- Keep each player a single self-contained HTML file (inline `<style>`/`<script>`, CDN `<script src>` for third-party libraries like Video.js and hls.js) so it can be hosted and iframed as-is with no build step.
- Configuration is passed via URL query parameters, not hardcoded — preserve this pattern when adding features.
- When no required query parameter is present, replace `document.body.innerHTML` with a plain-text error message rather than failing silently (see both existing files).
- README.md documents `index.html` under the name `iframe-player.html`; keep this naming in mind when cross-referencing the README with the actual file.

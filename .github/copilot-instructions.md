## Prizebooth — Copilot / AI agent instructions

This project is a small, static marketing/landing site for Prizebooth. The goal of these instructions is to help an AI coding agent make safe, useful edits quickly and consistently.

Rules of engagement (contract)
- Input: edits will be made to files under the repository root; most work will involve `index.html` and `assets/`.
- Output: small, focused patches that preserve the site's static nature (no new build system unless explicitly requested).
- Error modes: avoid changing server configuration or introducing runtime dependencies that can't be verified locally.

Quick big-picture
- This repo currently contains a single HTML entry: `index.html` and a static `assets/` folder for images.
- No package.json, build scripts, or tests were found. Treat this as a static site: editing HTML/CSS and replacing images is the common workflow.

Key files and patterns
- `index.html` — the single source of truth for markup and styling (uses large inline <style> block). Edit this file for layout/copy changes. Example selectors: `.hero`, `.feature-grid`, `.preview`.
- `assets/` — images referenced with absolute-root paths like `/assets/prizebooth_logo.png` and `/assets/mobile-ui-preview.png`. Always update both file and any references when replacing images.
- Note: the footer includes a PHP snippet (`<?php echo date('Y'); ?>`). This indicates the file may be served through a PHP-enabled host in production. Locally, a plain static server won't evaluate PHP — consider that when verifying edits.

Project-specific conventions
- Inline CSS: styles are embedded at the top of `index.html`. Keep visual tweaks local to that block unless adding multiple pages or a CSS file is requested.
- Absolute asset paths: image src values start with `/assets/...`. Preserve the leading slash for consistency unless moving the site to a subpath.
- Small, visual-first changes: most PRs will be copy updates, color tweaks, asset swaps, or small layout adjustments. Keep changes minimal and visual regressions easy to review.

How to run / verify locally
- Static preview: open `index.html` in the browser for basic checks.
- HTTP server (recommended for correct root-path handling):
  - Python 3: `python3 -m http.server 8000` (serve from repo root and open http://localhost:8000)
  - Or use VS Code Live Server extension to preview with correct paths.
- If you need to test the PHP year snippet, run via a PHP-enabled server (rename to `.php` or serve through PHP-FPM/Apache) — otherwise note that the snippet won't evaluate in a static server.

Examples the agent can act on
- Replace logo: update `/assets/prizebooth_logo.png` and keep the `<img>` attributes (width/height) consistent.
- Change hero copy: edit the <h1> and following <p> inside the .hero section. Keep CTA links intact (they point to the download section).
- Add a feature card: copy one `.feature-card` and edit its content; retain the `.feature-grid` structure so responsive grid rules still apply.

What to avoid
- Do not introduce a new build tool (Webpack/Vite) or package.json without the maintainer's consent.
- Avoid converting inline styles to a complex stylesheet across multiple files unless requested; it increases review burden.

If you need to extend the project
- For multi-file changes (adding JS or CSS), create `assets/scripts/` or `assets/styles/` and reference them from `index.html` with relative paths.
- When adding interactivity, prefer small, vanilla JS modules (no frameworks) unless the project owner asks for a migration.

If anything is unclear or you need a different scope (e.g., add CI, tests, or a build step), ask the maintainers before making large structural changes.

-- end of instructions

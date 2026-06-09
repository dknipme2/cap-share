# share/ — Public HTML Publish Surface

Files in this folder are **automatically published to GitHub Pages** via
`.github/workflows/pages.yml` on push to `dev`.

**Base URL:** `https://dknipme2.github.io/cdp-ip-foundation/`

**A file at `share/<name>.html` is served at `https://dknipme2.github.io/cdp-ip-foundation/<name>.html`.**

Build time: ~30-90s after push. Watch with `gh run list --workflow=pages.yml --limit 1`.

## Publish contract

Everything here is public and search-indexable. Before committing any file:

- **Company-agnostic.** No source-company names (CAP IP portability rule, CLAUDE.md domain).
- **Vendor-scrub.** No vendor product names ("Xponent" → "Rx data"). See CLAUDE.md FILE WRITING § vendor-agnostic.
- **No PII, no unreleased strategy, no internal architecture specifics.**
- **Self-contained HTML.** Inline CSS/JS, CDN for libraries (D3, etc.). No build step.
- **Assume the worst reader.** Meeting attendees, search engines, competitors.

## Why this exists

Repo is public, so every file is *technically* accessible via raw GitHub URLs. `share/` adds two things on top:

1. **Clean Pages URL** — no `htmlpreview` third party, no raw-URL CSP issues.
2. **Explicit IP-scrub contract** — opt-in publish surface. Files outside `share/` are accessible but not *promoted*; files inside `share/` assert they've been scrubbed.

## Workflow (agent-facing)

When the user asks for a shareable HTML link (for a meeting, demo, external share):

1. Write self-contained HTML to `share/<descriptive_name>.html`.
2. Apply the publish contract above. Grep for vendor/company names before commit.
3. `git add share/<name>.html && git commit -m "share: <what>" && git push origin dev`.
4. Watch Actions run (~60s).
5. Return `https://dknipme2.github.io/cdp-ip-foundation/<name>.html`.

## Cleanup

This folder accumulates over time. Delete stale meeting artifacts periodically — groundskeeper can flag files unchanged >90 days.

## When NOT to use share/

- Development/working HTML → `work/{PROJECT}/` (existing convention per CLAUDE.md).
- Internal architecture docs → `docs/` (existing internal content).
- Private handoff (time-limited, revocable access) → share/ is wrong; use a proper private hosting service.

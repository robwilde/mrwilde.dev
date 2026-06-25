# Design: Vendor the `terminal` theme into mrwilde.dev

- **Date:** 2026-06-25
- **Status:** Approved
- **Decision:** Convert `themes/terminal` from a git submodule into plain, tracked files in this repo (approach **A1** — keep the `themes/terminal/` directory). mrwilde.dev is the sole consumer of the theme.

## Context

The site uses the `terminal` Hugo theme, currently wired as a **git submodule** at `themes/terminal` pointing at the abandoned upstream `git@github.com:techbarrack/terminal-hugo-theme.git` (last upstream activity ~2 years ago). The user forked it to `https://github.com/robwilde/terminal-hugo-theme`.

Investigation surfaced a concrete fragility / data-loss risk in the current setup:

- The superproject records the submodule gitlink at commit **`ecce8013`**.
- The submodule **working tree is checked out 2 commits ahead at `d03898e`** (`557371c` "updated the blogs with images and shortcodes for YouTube. also have disqus running", `d03898e` "I made some small changes to improve the meta and other small items").
- `git branch -r --contains d03898e` is empty and the **fork is also only at `ecce8013`** → those 2 customization commits exist **only in this machine's local submodule clone**. A fresh `git submodule update --init` would reset the theme to `ecce8013` and silently drop them.

Additional facts that shape the migration:

- **Deployment is build-time-only for the theme.** Dokploy serves the committed, pre-built `public/`; the server runs no build step. The theme source is only needed when `hugo` runs locally/CI.
- **CSS is prebuilt and committed** inside the theme: `cd themes/terminal && npm run prod` writes `themes/terminal/assets/css/tailwind.css`, which Hugo then minifies + fingerprints. (Hugo does not compile Tailwind.)
- The project overrides **13 theme files** via `layouts/` (Hugo precedence: project `layouts/<path>` shadows `themes/terminal/layouts/<path>`).
- The local theme has **no `content/` directory**, so `ignoreFiles = ["themes/terminal/content"]` in `hugo.toml` is already a harmless no-op.
- `.gitmodules` also declares two **phantom** submodules (`themes/ananke`, `themes/introduction`) that are not checked out — leftover cruft.
- Docs (`AGENTS.md`, `CLAUDE.md`, `README.md`) describe the submodule and enforce a *"never edit the theme; override only in `layouts/`"* rule that exists **because** submodule edits are lost on update.

## Decision

Vendor the theme into the repo. For a single site, with a dead upstream, a build-time-only dependency, and a maintainer who already hand-edits the theme, a submodule (or Hugo Module) is overhead with a sharp edge. Vendoring yields one self-contained repo, allows direct theme edits, and permanently removes the out-of-sync-pointer data-loss trap.

**Approach A1 — keep `themes/terminal/` as vendored plain files.** `theme = 'terminal'` stays; override precedence, the Tailwind pipeline, and the build all stay identical. The only change is that the directory stops being a submodule.

## Alternatives considered

- **A2 — collapse the theme into the project root** (move theme `layouts/`/`assets/`/`static/` to root, drop `theme =`, merge the 13 overrides). Rejected for now: large, risky diff for no functional gain. YAGNI.
- **B — keep submodule, repoint to the fork.** Rejected: preserves theme/site separation we don't need, and retains the submodule friction (recursive clone, two-step commits, pointer sync) that is the source of the current breakage.
- **C — Hugo Modules (Go modules) importing the fork.** Rejected: versioning/reuse benefits are marginal for one site consuming a theme it fully owns; adds `go.mod` + Go-at-build-time conceptual overhead.

## Migration plan (outline)

1. **Safety net:** push the local-only commits (`557371c`, `d03898e`) to the fork (clean fast-forward from `ecce8013`). If push auth is unavailable, vendoring still preserves the content as committed files.
2. **De-submodule:** `git submodule deinit -f themes/terminal` → `git rm --cached themes/terminal` (drop the gitlink) → `rm -rf .git/modules/themes/terminal` and `rm -f themes/terminal/.git` → `git add themes/terminal` to re-track the **`d03898e`** working tree as normal files.
3. **Clean `.gitmodules`:** remove the `terminal` entry plus the phantom `ananke`/`introduction` entries; the file becomes empty → `git rm .gitmodules`.
4. **Update docs to the new reality:** `AGENTS.md`, `CLAUDE.md`, `README.md` — drop "submodule" framing and the "never edit the theme" rule (the theme is now owned and directly editable); repoint README's upstream link from `techbarrack` to the fork.

## What stays the same

- `hugo.toml` — `theme = 'terminal'` untouched; `ignoreFiles` left as a harmless no-op.
- The 13 `layouts/` overrides and their precedence.
- The committed `themes/terminal/assets/css/tailwind.css` and the `npm run prod` Tailwind step.
- **Dokploy deployment** — unaffected (serves prebuilt `public/`). Bonus: `git clone` no longer needs `--recursive`.

## Verification

- `themes/terminal/assets/css/tailwind.css` is actually tracked after `git add` (guard against the theme's nested `.gitignore` swallowing it).
- `hugo --buildDrafts` → 124 pages + fingerprinted CSS, zero errors.
- `hugo server --buildDrafts` → server starts at `localhost:1313`.
- **Fresh `git clone` to a temp dir without `--recursive` builds fully** — proves self-containment.

## Consequences

**Benefits:** one self-contained repo; theme files directly editable; data-loss trap removed; simpler clone; deployment unchanged.

**Tradeoffs:** theme no longer reusable across sites (acceptable — single site); theme history collapses to the vendored final state (`d03898e` content preserved); the fork becomes redundant (kept as an archive/backup).

## Deferred / optional (not in this migration)

- Drop `themes/terminal/images/` (`screenshot.png` + `tn.png` ≈ 2.3 MB of unused theme promo art).
- Archive the fork on GitHub once it holds the final snapshot.
- Merge the 13 `layouts/` overrides back into the now-owned theme (single edit location).

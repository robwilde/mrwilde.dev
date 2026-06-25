# Vendor the `terminal` Theme Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Convert `themes/terminal` from a git submodule into plain tracked files in the mrwilde.dev repo, with zero change to the rendered site.

**Architecture:** Approach A1 from `docs/plans/2026-06-25-vendor-terminal-theme-design.md`. Drop the submodule gitlink and the submodule's internal git dir, re-track the current theme working tree (`d03898e`) as ordinary files, delete the now-obsolete `.gitmodules`, and update docs that describe the submodule. `hugo.toml` (`theme = 'terminal'`), the 13 `layouts/` overrides, the prebuilt-Tailwind pipeline, and Dokploy deployment are all unchanged.

**Tech Stack:** Hugo extended v0.163.1, git submodules, bash, git.

---

## Preconditions & Notes

- **Working location:** the main checkout at `/var/home/mrwilde/Projects/MrWilde/Website/blogs/mrwilde.dev` (no dedicated worktree). Acceptable for this contained migration.
- **Pre-existing uncommitted change:** `hugo.toml` has uncommitted edits from a prior task (module.mounts `files` exclusion + `locale`). It is **unrelated** to this migration. Do **not** sweep it into these commits — every `git commit` below uses an explicit message and you should `git add` only the listed paths.
- **The theme working tree (`d03898e`) is the only copy of two local customization commits.** Task 1 backs it up before any destructive git op.
- Theme nested `.gitignore` keeps `node_modules`/`package-lock.json`/`.hugo_build.lock` ignored — retain it as a vendored file; it does not ignore `tailwind.css`.
- Skills: @verification-before-completion before claiming done; @atomic-commits for commit hygiene.

---

### Task 1: Capture baseline and back up the theme

**Files:** none modified.

**Step 1: Record the build baseline**

Run:
```bash
cd /var/home/mrwilde/Projects/MrWilde/Website/blogs/mrwilde.dev
rm -rf /tmp/vendor-baseline && hugo --buildDrafts -d /tmp/vendor-baseline 2>&1 | grep -iE 'pages|error'
```
Expected: a `Pages │ 124` row and **no** `ERROR` lines. Note the page count — every later build must match it.

**Step 2: Back up the live theme working tree**

Run:
```bash
rm -rf /tmp/terminal-backup && cp -r themes/terminal /tmp/terminal-backup && ls /tmp/terminal-backup
```
Expected: lists `layouts assets static hugo.yaml theme.toml ...`. This is the recovery copy of `d03898e`.

**Step 3 (optional, recommended): Push local theme commits to the fork as off-machine backup**

Run:
```bash
git -C themes/terminal remote add fork https://github.com/robwilde/terminal-hugo-theme.git 2>/dev/null || true
git -C themes/terminal push fork HEAD:master
```
Expected: a fast-forward push `ecce8013..d03898e  HEAD -> master`. If it fails on auth, that is OK — proceed; vendoring preserves the content regardless. (Do not block the migration on this.)

No commit in this task.

---

### Task 2: Convert the submodule to vendored files

**Files:**
- Modify (index/tree): `themes/terminal` (gitlink → real files)
- Delete: `.gitmodules`
- Delete: `themes/terminal/.git`, `.git/modules/themes/terminal`

**Step 1: Drop the gitlink from the index, keep files on disk**

Run:
```bash
git rm --cached themes/terminal
```
Expected: `rm 'themes/terminal'`. The directory and its files remain on disk.

**Step 2: Remove the submodule's internal git dir and pointer file**

Run:
```bash
rm -rf .git/modules/themes/terminal
rm -f themes/terminal/.git
```
Expected: no output. `themes/terminal` is now a plain directory.

**Step 3: Remove the submodule config section**

Run:
```bash
git config --remove-section submodule.themes/terminal 2>/dev/null || true
git config --get-regexp '^submodule\.' || echo "no submodule config remaining"
```
Expected: `no submodule config remaining`.

**Step 4: Delete the obsolete `.gitmodules`**

All three entries (`terminal` + phantom `ananke`/`introduction`) are obsolete.

Run:
```bash
git rm .gitmodules
```
Expected: `rm '.gitmodules'`.

**Step 5: Stage the theme as ordinary files**

Run:
```bash
git add themes/terminal
```
Expected: no error.

**Step 6: Verify the prebuilt CSS is tracked (the key guard)**

Run:
```bash
git ls-files themes/terminal/assets/css/tailwind.css
git check-ignore -v themes/terminal/assets/css/tailwind.css || echo "NOT ignored (good)"
```
Expected: the path `themes/terminal/assets/css/tailwind.css` is printed by `ls-files`, and `NOT ignored (good)`.

**Step 7: Verify no submodule remnants**

Run:
```bash
git submodule status; echo "exit=$?"
```
Expected: empty output (no submodules).

**Step 8: Build verification — must match baseline**

Run:
```bash
rm -rf /tmp/vendor-after && hugo --buildDrafts -d /tmp/vendor-after 2>&1 | grep -iE 'pages|error'
find /tmp/vendor-after -name '*.css'
```
Expected: `Pages │ 124` (same as Task 1), no `ERROR`, and a fingerprinted `css/tailwind.min.<sha>.css` present.

**Step 9: Commit**

```bash
git add themes/terminal .gitmodules
git commit -m "build: vendor terminal theme, drop git submodule"
```
Expected: commit succeeds; `git show --stat HEAD` lists the theme files added and `.gitmodules` deleted, and does **not** include `hugo.toml`.

---

### Task 3: Update docs to the vendored reality

**Files:**
- Modify: `AGENTS.md`
- Modify: `CLAUDE.md`
- Modify: `README.md`

**Step 1: `AGENTS.md` — remove submodule framing**

Re-read the file first (line numbers will have shifted). Apply these meaning-level edits:
- Intro (~lines 5-9): "...vendored as a git submodule that is itself a personal fork" → "...vendored directly into this repo (originally a fork of the abandoned `techbarrack/terminal-hugo-theme`)."
- Data-flow (~line 14): "`themes/terminal/` (submodule)" → "`themes/terminal/` (vendored)".
- Mermaid node (~line 22): "T[themes/terminal/ submodule]" → "T[themes/terminal/ vendored]".
- File-map row (~line 47): "Active theme submodule — **do not edit** (changes lost on submodule update)" → "Vendored theme — **editable directly** (no submodule; project `layouts/` still override per-file)."
- CSS-pipeline / tailwind-build mentions (~lines 33-34, 63-64, 117-118, 127): keep paths as-is (`themes/terminal/...` is still correct); only drop any "submodule" wording if present.

**Step 2: `CLAUDE.md` — Architecture Notes**

Line ~62: "**Theme**: Git submodule at `themes/terminal` - avoid modifying theme files directly" → "**Theme**: Vendored at `themes/terminal` (forked from the abandoned upstream); edit directly. Project `layouts/` still override theme files per-file."

**Step 3: `README.md`**

- Line ~8: repoint the theme link from `https://github.com/techbarrack/terminal-hugo-theme` to `https://github.com/robwilde/terminal-hugo-theme` and note it is vendored.
- Line ~55: "`├── themes/terminal/   # Theme (git submodule)`" → "`├── themes/terminal/   # Theme (vendored)`".

**Step 4: Sanity check no stale "submodule" references remain**

Run:
```bash
grep -rin 'submodule' AGENTS.md CLAUDE.md README.md || echo "clean"
```
Expected: `clean` (or only intentional historical mentions you chose to keep).

**Step 5: Commit**

```bash
git add AGENTS.md CLAUDE.md README.md
git commit -m "docs: reflect vendored theme (no submodule)"
```

---

### Task 4: Final self-containment verification

**Files:** none modified.

**Step 1: Fresh clone WITHOUT `--recursive`**

Run:
```bash
rm -rf /tmp/clone-test && git clone . /tmp/clone-test
test -e /tmp/clone-test/.gitmodules && echo "UNEXPECTED .gitmodules" || echo "no .gitmodules (good)"
test -f /tmp/clone-test/themes/terminal/hugo.yaml && echo "theme files present (good)"
```
Expected: clone succeeds, `no .gitmodules (good)`, `theme files present (good)`.

**Step 2: Build the clone with no submodule init**

Run:
```bash
cd /tmp/clone-test && hugo --buildDrafts 2>&1 | grep -iE 'pages|error'; cd -
```
Expected: `Pages │ 124`, no `ERROR`. Proves the repo is self-contained.

**Step 3: Clean up temp dirs**

Run:
```bash
rm -rf /tmp/vendor-baseline /tmp/vendor-after /tmp/clone-test /tmp/terminal-backup
```
(Keep `/tmp/terminal-backup` until you are satisfied; remove last.)

No commit.

---

## Done criteria

- `git submodule status` empty; `.gitmodules` gone; `.git/modules/themes/terminal` gone.
- `themes/terminal/` tracked as ordinary files incl. `assets/css/tailwind.css`.
- `hugo --buildDrafts` and a fresh non-recursive clone both build 124 pages with CSS and no errors.
- Docs contain no stale "submodule" framing.
- The unrelated `hugo.toml` working-tree change was not folded into these commits.

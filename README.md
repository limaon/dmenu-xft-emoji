# dmenu-xft-emoji

**dmenu** with native color-font (emoji) rendering and automatic CJK fallback (漢字、かな、カタカナ). Built on top of dmenu 5.4 with a small patch set and a cascading font stack (`Terminus → Noto Sans CJK JP → Noto Color Emoji → monospace`).

## Features

* **Emoji support** via the *allow‑color‑font* patch.
* Seamless fallback for Chinese, Japanese and Korean glyphs.
* Lean, easy‑to‑edit `config.h`.
* Arch Linux package (`PKGBUILD`) that **provides dmenu** and can fully replace the stock build.
* Upstream repo is tagged, making builds reproducible.

## Installation

### AUR (recommended)

```bash
paru -S dmenu-xft-emoji      # or yay / pikaur / aurutils …
```

> **Runtime deps**: `libxft`, `libxinerama`, `fontconfig`
> **Suggested fonts**: `terminus-font`, `noto-fonts`, `noto-fonts-cjk`, `noto-fonts-emoji`

## Quick test

```bash
echo -e "Hello 👋\nこんにちは" | dmenu -i
```

If both the emoji and the Kanji render, fallback is working.

## Repository Structure

```mermaid
graph LR
    subgraph "Remotes"
        U[upstream<br/>suckless.org/dmenu]
        G[github<br/>limaon/dmenu-xft-emoji]
        A[aur<br/>aur.archlinux.org]
    end

    U -->|"git fetch"| G
    G -->|"sync subset"| A

    style U fill:#e1f5fe
    style G fill:#e8f5e9
    style A fill:#fff3e0
```

## Updating from Upstream

This project tracks the official dmenu from [suckless.org](https://git.suckless.org/dmenu).

### Initial Setup (one-time)

```bash
git remote add upstream https://git.suckless.org/dmenu
```

### Sync with Upstream

```bash
# Fetch latest changes
git fetch upstream

# Rebase my commits onto latest upstream
git rebase --rebase-merges upstream/master

# If conflicts occur:
git status                    # see conflicted files
git checkout --ours FILE      # keep my changes (or --theirs for upstream)
git add FILE
git rebase --continue

# Update patch file if drw.c changed
git diff upstream/master -- drw.c > dmenu-allow-color-font-5.4.diff
git add dmenu-allow-color-font-5.4.diff
git commit -m "chore: Update patch for upstream"

# Push to GitHub (force needed after rebase)
git push github master --force
```

### How Rebase Works

```mermaid
---
title: Sync Process - Before & After Rebase
---
gitGraph
    commit id: "5.3"
    commit id: "5.4" tag: "v5.4"
    branch upstream
    checkout upstream
    commit id: "fix-1"
    commit id: "fix-2"
    checkout main
    commit id: "color-font patch"
    commit id: "custom config"
    commit id: "documentation"
```

**Before rebase:** My patches are based on an older upstream version.

```mermaid
---
title: After git rebase --rebase-merges upstream/master
---
gitGraph
    commit id: "5.3"
    commit id: "5.4" tag: "v5.4"
    commit id: "fix-1"
    commit id: "fix-2"
    commit id: "color-font patch"
    commit id: "custom config"
    commit id: "documentation"
```

**After rebase:** My patches are replayed on top of the latest upstream.

### Updating Checksums

After modifying patch or config files:

```bash
sha256sum dmenu-allow-color-font-*.diff config.h
# Update PKGBUILD sha256sums array accordingly
```

## Syncing to AUR

Since AUR doesn't allow subdirectories (like `.github/`), use this workflow to sync updates:

```mermaid
---
title: AUR Sync Workflow
---
flowchart TB
    subgraph GitHub["GitHub (master)"]
        G1[PKGBUILD]
        G2[config.h]
        G3[*.diff]
        G4[README.md]
        G5[".github/ (ignored)"]
        G6["dmenu.c, drw.c (source)"]
    end

    subgraph AUR["AUR (master)"]
        A1[PKGBUILD]
        A2[config.h]
        A3[*.diff]
        A4[README.md]
        A5[.SRCINFO]
    end

    G1 --> A1
    G2 --> A2
    G3 --> A3
    G4 --> A4

    style G5 fill:#d32f2f,stroke-dasharray: 5 5,color:#ffffff
    style G6 fill:#d32f2f,stroke-dasharray: 5 5,color:#ffffff
    style A5 fill:#2e7d32,color:#ffffff
```

```bash
# Create clean branch from AUR
git checkout -b aur-sync aur/master

# Copy only AUR-compatible files from master
git checkout master -- PKGBUILD config.h dmenu-allow-color-font-*.diff README.md

# Update .SRCINFO (must match PKGBUILD)
# Edit .SRCINFO with new version, checksums, and source files

# Commit and push to AUR
git add -A
git commit -m "Update to dmenu X.Y"
git push aur aur-sync:master

# Clean up
git checkout master
git branch -D aur-sync
```

### .SRCINFO Format

The `.SRCINFO` file must be manually updated to match `PKGBUILD`:

```
pkgbase = dmenu-xft-emoji
	pkgdesc = dmenu with color-font (emoji) and CJK fallback support
	pkgver = 5.4
	pkgrel = 1
	url = https://tools.suckless.org/dmenu/
	arch = x86_64
	license = MIT
	depends = libxft
	depends = libxinerama
	depends = fontconfig
	optdepends = terminus-font: bitmap Terminus for X11
	source = https://dl.suckless.org/tools/dmenu-5.4.tar.gz
	source = dmenu-allow-color-font-5.4.diff
	source = config.h
	sha256sums = <dmenu-tarball-checksum>
	sha256sums = <patch-checksum>
	sha256sums = <config-checksum>

pkgname = dmenu-xft-emoji
```

## Contributing

1. **Fork** and create a topic branch: `git checkout -b feature/name`.
2. Commit in logical chunks with clear English messages.
3. Open a pull request against `main`.
4. Use the issue tracker for questions or bug reports.

## License

Released under the MIT License. See `LICENSE` for details.

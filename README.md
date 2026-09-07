# m3e-skill — Material 3 Expressive Master Skill

A deep, reusable **Material 3 Expressive design + implementation expert** for Android — usable as an agent skill in **OpenCode, Claude Code, Cursor**, and any `skills`-compatible agent.

Built from thorough source study of two catalog repos:
- [`meticha/material-3-expressive-catalog`](https://github.com/meticha/material-3-expressive-catalog) (46 .kt, Navigation 3, BOM 2025.06)
- [`emertozd/Compose-Material-3-Expressive-Catalog`](https://github.com/emertozd/Compose-Material-3-Expressive-Catalog) (78 .kt / 49 samples, M3 1.5.0-alpha26, from AOSP)

## Install

### Option A — skills CLI (recommended)

```sh
npx skills add https://github.com/JusDots-Devs/m3e-skill
# or shorthand
npx skills add JusDots-Devs/m3e-skill -g -a opencode -y
```

### Option B — direct npx (no extra install)

```sh
npx https://github.com/JusDots-Devs/m3e-skill
npx https://github.com/JusDots-Devs/m3e-skill upgrade
npx https://github.com/JusDots-Devs/m3e-skill uninstall
```

The installer copies `skills/material-3-expressive/` to `~/.config/opencode/skills/`, `~/.opencode/skills/`, `~/.claude/skills/`, and `~/.cursor/skills/` (whichever exist).

### Option C — manual

```sh
git clone https://github.com/JusDots-Devs/m3e-skill
cp -r m3e-skill/skills/material-3-expressive ~/.config/opencode/skills/
```

## What the skill does

The skill behaves like a **Material 3 Expressive design expert + Jetpack Compose implementation expert**:

- Designs production screens (notes, settings, music player, etc.) with specific Compose APIs
- Reviews/audits existing Compose screens for M3 Expressive violations
- Chooses components (when to use standard vs expressive variant), theming, adaptive layouts, motion, and a11y
- Distinguishes **M3 vs M3 Expressive vs generic modern UI**; optimizes for **clarity → hierarchy → usability → consistency → expressiveness**

## Contents

```
skills/material-3-expressive/
  SKILL.md                      # main expert instruction set (decision framework, checklist)
  references/
    FOUNDATIONS.md              # M3 vs M3E, color/typography/shape/elevation/spacing
    COMPONENTS.md               # full catalog + selection rules (buttons, FABs, toolbars, etc.)
    LAYOUTS.md                  # expressive + adaptive (WindowSizeClass, NavigationSuite)
    MOTION.md                   # shapes() morph, wavy, FAB morph, pull-to-refresh
    THEMING.md                  # 4-way colorScheme, dynamic color, dark/light
    ACCESSIBILITY.md            # TooltipBox, semantics, touch targets
    PATTERNS.md                 # production recipes (notes, settings, music, tablet, M2→M3E)
    ANTI_PATTERNS.md            # fragile patterns to avoid (with source citations)
    COMPOSE_IMPLEMENTATION.md   # exact APIs + copy-paste snippets
```

## Quick examples

The skill can handle:

> “Design a beautiful Material 3 Expressive notes screen.” → `CenterAlignedTopAppBar` + `LazyVerticalGrid(Fixed 2→3 adaptive)` of `ElevatedCard(surfaceContainer)` + `LargeExtendedFloatingActionButton(expanded=!collapsed)`

> “Review this Compose screen for M3 Expressive violations.” → checklist: `colorScheme` roles only, `MaterialTheme.typography`, `ButtonDefaults.shapes()`, `surfaceContainer*` hierarchy, `currentWindowAdaptiveInfo`, `TooltipBox`

> “Make this screen more expressive without clutter.” → apply `shapes()` morph to buttons + one hero (`CircularWavyProgressIndicator` or `vibrantFloatingToolbarColors`), keep secondaries calm (≤30% expressive)

## Requirements

- Node ≥18 for the installer (skills CLI requires its own deps)
- Target apps: `androidx.compose.material3:material3:1.5.0-alpha26`, Compose BOM `2026.08.00` / `2025.06` (covers M3 1.4–1.5), `compileSdk 36–37`
- Skill itself has zero runtime deps — it's Markdown that agents read

## License

- This skill: **Apache-2.0** (same as source catalogs)
- Source catalogs retain their own Apache-2.0 licenses; this skill summarizes patterns and does not copy large source blocks
- Attribution preserved in `SKILL.md` and `references/`

## References

- Material 3 spec: https://m3.material.io
- Catalog sources linked above

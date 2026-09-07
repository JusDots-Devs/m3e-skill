<div align="center">

# m3e-skill

### Material 3 Expressive — Master Skill for Android

*Your agent, now a Material 3 Expressive design expert.*

[![Version](https://img.shields.io/badge/version-1.0.0-7c4dff?style=flat-square)](https://github.com/JusDots-Devs/m3e-skill/releases/tag/v1.0.0)
[![License](https://img.shields.io/badge/License-Apache_2.0-4285f4?style=flat-square)](LICENSE)
[![M3](https://img.shields.io/badge/Material_3-Expressive-ff7043?style=flat-square)](https://m3.material.io)
[![Compose](https://img.shields.io/badge/Compose-BOM_2026.08-00c853?style=flat-square)](https://developer.android.com/jetpack/compose)
[![Skills](https://img.shields.io/badge/skills-OpenCode_+_Claude_+_Cursor-ffab00?style=flat-square)](https://skills.sh)

**Design, review, and implement polished Android UIs in Jetpack Compose — with a single prompt.**

</div>

<div align="center">

```sh
npx https://github.com/JusDots-Devs/m3e-skill
```

*or* `npx skills add JusDots-Devs/m3e-skill` · zero deps · 10 seconds · works everywhere

</div>

---

## TL;DR

**The Problem:** Material 3 Expressive is gorgeous but fragmented. Google's catalogs are exhaustive demos, not production guides. Teams waste days guessing *when* to use `ButtonDefaults.shapes()` vs. when to stay calm, which color roles actually matter, or how to adapt a phone screen for tablets without breaking hierarchy.

**The Solution:** A single reusable skill that makes your coding agent a **Material 3 Expressive expert + Compose implementation expert** — trained on real source from both official catalogs, distilled into opinionated rules, exact APIs, and copy-paste snippets.

### Why m3e-skill?

| What you get | Why it matters |
|---|---|
| **One hero, calm rest** — clarity → hierarchy → usability → consistency → **expressiveness** | Stops “everything rounded and wavy” noise; expressive becomes intentional, not exhausting |
| **Exact APIs, not vibes** — `ButtonDefaults.shapes()`, `CircularWavyProgressIndicator`, `FloatingToolbarDefaults.ScreenOffset` | No invented props; every recommendation traces to verified source |
| **Production recipes** — notes, settings, music player, tablet adapt, M2→M3E migration | Not just gallery demos; end-to-end screens you actually ship |
| **Adaptive out of the box** — `currentWindowAdaptiveInfo()` → `maxItemCount`, rail/bar, pane scaffolds | Phone → tablet → foldable without rethinking layout |
| **A11y built in** — `TooltipBox`, `semantics{role, stateDescription}`, 48dp, focus order | Expressive never at the cost of accessibility |

---

## Quick Example

Ask your agent — the skill activates automatically:

```sh
# In OpenCode / Claude Code / Cursor, just prompt:

> "Design a beautiful Material 3 Expressive notes screen."

# Agent delivers:
#  CenterAlignedTopAppBar + LazyVerticalGrid(Fixed 2→3 adaptive)
#  of ElevatedCard(surfaceContainer, 16.dp) + LargeExtendedFloatingActionButton(expanded=!collapsed)

> "Review this Compose screen for M3 Expressive violations."

# Agent audits:
#  colorScheme roles only? typography scale? shapes() morph?
#  surfaceContainer* hierarchy? TooltipBox on icons? adaptive maxItemCount?

> "Make this screen more expressive without clutter."

# Agent: one morph + one hero (wavy OR vibrant), rest calm — ≤30% expressive
```

<details>
<summary><strong>More prompts the skill handles</strong></summary>

- *“What component replaces this custom button?”* → tables of standard vs expressive variants
- *“Which M3 Expressive pattern fits this interaction?”* → decision framework + trace to catalog file
- *“Refactor this old Material 2 UI into modern M3 Expressive.”* → 6-step migration recipe
- *“Audit this screen against M3 Expressive principles.”* → 8-point checklist
- *“Implement this Figma in Compose.”* → exact composables, shape tokens, motion specs

</details>

---

## Design Philosophy

The skill optimizes for **clarity → hierarchy → usability → consistency → expressiveness** — in that order.

| Principle | In practice |
|---|---|
| **Clarity first** | Every expressive treatment must improve comprehension. If it adds noise, it’s removed. |
| **One hero per screen** | A `LargeExtendedFloatingActionButton`, wavy indicator, or `vibrantFloatingToolbarColors` — never all three. |
| **Intentional asymmetry** | A tight grid against a large rounded FAB; a flat card list under a wavy progress. Contrast, not decoration. |
| **Surface over shadow** | `surface → surfaceContainerLow → surfaceContainer → surfaceContainerHigh` encodes depth, not extra borders. |
| **Motion = meaning** | Press morph, chevron `animateFloatAsState(180f)`, `ProgressAnimationSpec` — only on state change. |

---

## M3 vs M3 Expressive vs Generic

| Dimension | Material 3 (baseline) | **m3e — M3 Expressive** | Generic “modern” UI |
|---|---|---|---|
| Shapes | `RoundedCornerShape(8–16dp)` static | **Morphing** `ButtonDefaults.shapes()` on press | Arbitrary radii, no tokens |
| Color | `colorScheme` roles | Roles + **vibrant accents** + dynamic color | Hard-coded hex |
| Motion | Subtle `ProgressAnimationSpec` | **Wavy**, `animateIcon` morph, spring | Generic fade/slide |
| Density | Balanced | **Spacious hero + calm secondaries** | Uniformly dense or airy |
| Signal | Consistent | **Expressive = emphasis, not default** | Decorative |

> *Rounded corners alone ≠ expressive.* The skill teaches the difference.

---

## Installation

### 1 · One-liner (fastest)

```sh
npx https://github.com/JusDots-Devs/m3e-skill
```

Installs to every detected agent (`~/.config/opencode/skills/`, `~/.opencode/skills/`, `~/.claude/skills/`, `~/.cursor/skills/`). Manage later:

```sh
npx https://github.com/JusDots-Devs/m3e-skill upgrade    # reinstall / update
npx https://github.com/JusDots-Devs/m3e-skill uninstall  # remove
```

### 2 · skills CLI (recommended for teams)

```sh
npx skills add https://github.com/JusDots-Devs/m3e-skill          # interactive
npx skills add JusDots-Devs/m3e-skill -g -a opencode -y           # CI-friendly, global
npx skills add JusDots-Devs/m3e-skill --skill material-3-expressive --list  # preview
```

Browse & discover: [skills.sh](https://skills.sh)

### 3 · Manual

```sh
git clone https://github.com/JusDots-Devs/m3e-skill
cp -r m3e-skill/skills/material-3-expressive ~/.config/opencode/skills/
# restart your agent
```

**Requirements:** Node ≥18 for installer only; the skill itself is Markdown — zero runtime deps. Targets `androidx.compose.material3:material3:1.5.0-alpha26`, Compose BOM `2026.08` / `2025.06`, `compileSdk 36–37`.

---

## Quick Start

```sh
# 1. Install (pick one method above)
npx https://github.com/JusDots-Devs/m3e-skill

# 2. Restart your agent (OpenCode / Claude Code / Cursor)

# 3. Prompt — skill auto-activates on keywords:
#    "material 3", "material you", "expressive", "compose ui", "polish", "audit"
> Design a Material 3 Expressive settings screen that adapts to tablets

# 4. Iterate:
> Review it for a11y and make the primary action more expressive
```

---

## What’s Inside

```
skills/material-3-expressive/
├── SKILL.md                      # decision framework + review checklist + traceability
└── references/
    ├── FOUNDATIONS.md            # color roles (22), typography, shape tokens, elevation
    ├── COMPONENTS.md             # full catalog — buttons → FABs → toolbars → cards → chips …
    ├── LAYOUTS.md                # expressive layout + adaptive (WindowSizeClass, NavigationSuite)
    ├── MOTION.md                 # shapes() morph, wavy, FAB icon morph, pull-to-refresh
    ├── THEMING.md                # 4-way colorScheme, dynamic color, dark/light
    ├── ACCESSIBILITY.md          # TooltipBox, semantics, touch targets, focus order
    ├── PATTERNS.md               # production recipes — notes, settings, music, tablet, M2→M3E
    ├── ANTI_PATTERNS.md          # what to avoid (unreachable theme branch, 64.dp, missing tooltips…)
    └── COMPOSE_IMPLEMENTATION.md # exact APIs + copy-paste snippets (210 lines)
```

Built from **deep source study**, not screenshots:

| Catalog | Scope | Stack |
|---|---|---|
| [`meticha/material-3-expressive-catalog`](https://github.com/meticha/material-3-expressive-catalog) | 46 .kt, 10 component families | Navigation 3 + Hilt, BOM 2025.06 |
| [`emertozd/Compose-Material-3-Expressive-Catalog`](https://github.com/emertozd/Compose-Material-3-Expressive-Catalog) | 78 .kt, 49 samples, ~230 composables | M3 1.5.0-alpha26, BOM 2026.08, from AOSP |

Cross-referenced for agreements, divergences (e.g., `Slider(value)` vs `rememberSliderState()`), and anti-patterns.

---

## Architecture

How the skill guides your agent:

```
┌─────────────────────────────────────────────────────────────────┐
│                        Your Prompt                               │
│  "Design / Review / Adapt / Migrate / Implement …"              │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    SKILL.md (router)                             │
│  principles → decision framework → checklist → references        │
└─────────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                     ▼
┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
│  Foundations     │ │  Components      │ │  Patterns        │
│  M3 vs M3E       │ │  Buttons, FABs   │ │  Notes, Settings │
│  Color / Shape   │ │  Toolbars, Bars  │ │  Music, Tablet   │
│  Type / Motion   │ │  Progress, Lists │ │  M2 → M3E        │
└──────────────────┘ └──────────────────┘ └──────────────────┘
        │                     │                     │
        └─────────────────────┼─────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│              COMPOSE_IMPLEMENTATION.md                            │
│  exact imports + snippets → paste-ready Compose code             │
│  traced to source file + line (e.g., emertozd/ButtonSamples.kt) │
└─────────────────────────────────────────────────────────────────┘
```

---

## Troubleshooting

### Skill doesn’t activate

The skill triggers on keywords (`material 3`, `expressive`, `compose ui`, `audit`, `polish`). If it doesn’t fire, name it explicitly:

```
> Using the material-3-expressive skill, design …
```

### Installed but not found by `npx skills list`

Ensure you installed to the right scope:

```sh
npx skills list              # project + global
npx skills list -g           # global only
npx skills add JusDots-Devs/m3e-skill -g -a opencode -y  # force global + agent
```

### `npx https://…` fails with network error

The repo is public and requires no token. Retry; if behind a proxy, use the manual method. For private forks, set `GITHUB_TOKEN` or `gh auth login`.

### Compose API not found (`ButtonDefaults.shapes()` etc.)

You’re on M3 <1.4. Update:

```kotlin
implementation("androidx.compose.material3:material3:1.5.0-alpha26")
```

### Dark theme looks wrong (status bar icons invisible)

Add the `WindowCompat` side-effect from `THEMING.md` — the skill’s 4-way `colorScheme` branch fixes this.

---

## Limitations

| Area | Status | Note |
|---|---|---|
| M3 Expressive alpha | `ExperimentalMaterial3ExpressiveApi` | APIs may change; skill scopes `@OptIn` to wrappers |
| `WideNavigationRail` | Experimental | Gated; use `NavigationSuiteScaffold` as stable fallback |
| `expressiveDarkColorScheme` | Not yet in catalogs | Falls back to `darkColorScheme` |
| Slider | `rememberSliderState()` (M3 1.5) | Old `Slider(value)` is legacy — skill documents both |
| Catalog coverage | 49 samples, not 100% of M3 | Prioritizes production-relevant patterns; PRs welcome |

---

## FAQ

**Is this just “rounded + animated = expressive”?** — No. See `FOUNDATIONS.md`. Expressive requires `shapes()` morph + at least one wavy/vibrant/large hero, used sparingly.

**Does it replace the catalogs?** — No. It distills them into an agent-readable expert. Link to originals for full demos.

**Will it over-decorate dense screens?** — The skill enforces restraint: settings/tables stay calm, only interaction morph.

**Does it handle tablet/foldable?** — Yes. `LAYOUTS.md` + adaptive recipes drive `maxItemCount`, `NavigationSuiteScaffold`, `ListDetailPaneScaffold`.

**Can I use it without Node?** — Yes. Manual copy needs no Node; Node is only for the one-line installer.

**How do I update?** — `npx skills update` or `npx https://github.com/JusDots-Devs/m3e-skill upgrade`.

---

## License

**Apache-2.0** — same as source catalogs. This skill summarizes patterns; it does not copy large source blocks. Attribution preserved in `SKILL.md` and `references/`.

Source licenses: [`meticha`](https://github.com/meticha/material-3-expressive-catalog/blob/main/LICENSE) · [`emertozd`](https://github.com/emertozd/Compose-Material-3-Expressive-Catalog) (AOSP)

---

<div align="center">

Built with care by [JusDots-Devs](https://github.com/JusDots-Devs) · Feedback at [opencode#issues](https://github.com/anomalyco/opencode)

`npx https://github.com/JusDots-Devs/m3e-skill`

</div>

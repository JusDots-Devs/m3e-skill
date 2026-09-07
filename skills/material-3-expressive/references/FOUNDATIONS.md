# Foundations — Material 3 Expressive

## M3 vs M3 Expressive vs Generic Modern UI

| Dimension | Material 3 (baseline) | Material 3 Expressive | Generic modern (not M3E) |
|---|---|---|---|
| Shapes | `Shapes` tokens, `RoundedCornerShape(8–16dp)`, static | **Morphing shapes**: `ButtonDefaults.shapes()` / `IconButtonDefaults.shapes()` animate on press; `CircleShape` ↔ `RoundedCornerShape` transitions; wavy indicators | Arbitrary large radius, glassmorphism, no token system |
| Color | `colorScheme` roles, `surfaceContainer*` hierarchy | Same roles + **vibrant accents** (`FloatingToolbarDefaults.vibrantFloatingToolbarColors`, custom brand scheme `#006E2C` in `emertozd/Theme.kt`) | Hard-coded hex, no role mapping |
| Motion | `ProgressAnimationSpec` subtle | **Expressive motion**: wavy `CircularWavyProgressIndicator`, `animateIcon` morph, `animateFloatingActionButton`, spring `DampingRatioNoBouncy, StiffnessVeryLow` | Generic fade/slide, no spec |
| Density | Balanced | **Spacious hero + calm secondaries** — one large element per screen | Uniformly dense or uniformly airy |
| Typography | Type scale `display/label/title/body` | Same scale, **emphasis via size token** `textStyleFor(ExtraLargeContainerHeight)` | Custom font sizes per screen |

**Rule:** If the UI uses `MaterialExpressiveTheme` + `shapes()` morph + at least one wavy/vibrant/large hero, it is M3 Expressive. Rounded corners alone ≠ expressive.

## Color

- **Roles** (Repo-C `ColorComposables.kt` `ColorRolesScreen` catalogs 22): primary/onPrimary/primaryContainer/onPrimaryContainer, secondary/tertiary families, error family, `surface / surfaceVariant / surfaceContainer / surfaceContainerHigh / surfaceContainerHighest / surfaceDim / surfaceBright / inverseSurface / surfaceContainerLowest`, `outline / outlineVariant`, `scrim`.
- Use roles only via `MaterialTheme.colorScheme.*`. Never hard-code except brand override: `LightCustomColorScheme` (`emertozd/library/ui/theme/Theme.kt`) — permitted only when `ColorMode.Custom`.
- **Dynamic color**: `dynamicLightColorScheme(context)` / `dynamicDarkColorScheme(context)` (Android 12+). Fallback to `expressiveLightColorScheme()` when no dynamic. Repo-C bug: `when { isDarkTheme -> ...; !isDarkTheme -> ...; else -> ... }` unreachable — never use this pattern; use `when { isDark && dynamic -> dynamicDark; isDark -> darkExpressive; dynamic -> dynamicLight; else -> expressiveLight }` (see `THEMING.md`).
- **Vibrant**: `FloatingToolbarDefaults.vibrantFloatingToolbarColors()` for toolbar hero; not for every surface.

## Typography

- Type scale from `MaterialTheme.typography` (`displayLarge..labelSmall`). Repo-C `Type.kt` only overrides `bodyLarge(16sp/24sp/0.5sp)` — otherwise default. Repo-E `TypographySamples.kt` demos full scale + custom `FontFamily` via `GoogleFonts`.
- Size-driven text style: `ButtonDefaults.textStyleFor(size)` maps `ExtraSmall/Medium/Large/ExtraLargeContainerHeight` → correct `labelLarge` etc. Do not set `fontSize` manually on buttons.
- Emphasis via type scale jump (e.g., `headlineMedium` for empty-state hero), not weight hacking.

## Shapes & Corner Treatments

- **Interactive morph**: `ButtonDefaults.shapes()` / `IconButtonDefaults.shapes()` returns `MorphShape` that animates `CircleShape ↔ RoundedCornerShape` on press. Cheapest expressive win — apply to all buttons/iconButtons. Verified: `meticha/ButtonComposables.kt`, `emertozd/ButtonSamples.kt`.
- **Tokens**: `RoundedCornerShape(4/12/16/28.dp)` for cards/sheets. `GroupLargeCorner 28.dp` outer / `GroupSmallCorner 6.dp` inner for segmented lists (`segmented-connect-list` skill). `CircleShape` for avatars/chips/containers.
- **Square opt-out**: `ButtonDefaults.squareShape` when shape must not morph (rare).
- Never mix arbitrary radii (e.g., `13.dp`) — stick to 4/12/16/28 or tokens.

## Elevation & Surface Hierarchy

- Expressive prefers **tonal elevation** (color overlay via `surfaceContainer*`) over shadow. Levels: `surface → surfaceContainerLow → surfaceContainer → surfaceContainerHigh → surfaceContainerHighest`.
- `ElevatedCard` / `ElevatedButton` for lifted content; `Card(surfaceContainer)` for flat grouping; `surfaceContainerHighest` for toolbar scrim. See `emertozd/CardSamples.kt`, `meticha/GridTile.kt` (`surfaceContainer` + `RoundedCornerShape(16.dp)`).

## Spacing & Density

- 8dp grid. Repo-C `FloatingToolbarDefaults.ScreenOffset` = edge inset token; `ButtonGroupDefaults.ConnectedSpaceBetween` = inter-button gap.
- Spacious hero needs 16–24dp padding; dense lists use 12dp horizontal / 8dp vertical. Avoid `64.dp` hard-code — use `contentPaddingFor(size)` tokens.

## Iconography

- Material Symbols (outlined ↔ filled morph on toggle: `if(checked) Filled.Lock else Outlined.Lock` — `meticha/ButtonComposables.kt`).
- Icon size tokens: `ButtonDefaults.iconSizeFor(size)`, `SplitButtonDefaults.LeadingIconSize/TrailingIconSize`, `FloatingActionButtonDefaults.LargeIconSize`.

## Motion (summary — detail in MOTION.md)

- Press morph via `shapes()`, rotation `animateFloatAsState(180f)` with `graphicsLayer(rotationZ)`, wavy via `ProgressIndicatorDefaults.ProgressAnimationSpec` + `Stroke(8.dp, Round)`.

## State Layers

- Built-in via M3 ripple (`LocalRippleThemeConfiguration`). Repo-E `Theme.kt` toggles `InsetFocusRing` vs `Opacity`. Do not implement custom ripple.

## Source notes

- Color roles catalog: `meticha/components/colors/ColorComposables.kt: ColorRolesScreen` (22 roles, hex + `Aa` contrast).
- Shape tokens + brand color: `emertozd/library/ui/theme/Theme.kt`, `emertozd/MaterialShapesSamples.kt`.
- Icon morph: `meticha/components/buttons/ButtonComposables.kt`.

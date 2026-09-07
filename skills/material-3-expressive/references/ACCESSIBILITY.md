# Accessibility — Material 3 Expressive

## Non-negotiable (never simplify away)

- 48dp minimum touch target: use `ButtonDefaults.heightIn(size)` / `IconButton` defaults — never shrink below.
- Real `contentDescription` via `stringResource`, not `"Localized description"` placeholder (Repo-C violates this — lint will fail).
- `TooltipBox` on every icon-only control for tablet/desktop and for screen-reader discoverability.
- `semantics { role, stateDescription, liveRegion, paneTitle }` where state is not visible.

## Icon buttons — tooltip + a11y pattern (Repo-E canonical)

```kotlin
TooltipBox(
    positionProvider = TooltipDefaults.rememberTooltipPositionProvider(TooltipAnchorPosition.Above),
    state = rememberTooltipState(),
    tooltip = {
        PlainTooltip(
            modifier = Modifier.semantics { liveRegion = LiveRegionMode.Assertive; paneTitle = title }
        ) { Text(title) }
    }
) {
    IconButton(onClick = onClick) {
        Icon(imageVector, contentDescription = title) // same string
    }
}
```
Repo-E adds `focusProperties { canFocus = expanded }` on toolbar items and `zIndex(1f)` so toolbar is focusable before content. TODO `b/496338253` documents `liveRegion` workaround — keep until fixed.

## Semantics for toggles & groups

```kotlin
// Single-select group
Row(Modifier.selectableGroup()) {
    ConnectedButtonGroup {
        ToggleButton(checked, Modifier.semantics { role = Role.RadioButton }) { ... }
    }
}
// Expandable
Icon(KeyboardArrowDown, Modifier.semantics { stateDescription = if(expanded) "Expanded" else "Collapsed" })
```

## FAB menu a11y

- `Modifier.semantics { isTraversalGroup = true }` on menu container, `traversalIndex = -1f` on scrim, `customActions = listOf(CustomAccessibilityAction("Close menu") { expanded = false; true })`, `BackHandler` for focus escape. Ref: `meticha/FabMenuComposable.kt`.

## Focus order

- Toolbar/FAB menu must be composed **before** `LazyColumn` + `zIndex(1f)` so TalkBack hits controls before list items. Repo-C after-list placement is anti-pattern.

## Text & contrast

- Type scale from theme (not ad-hoc sizes) keeps minimum contrast. Verify error/surface roles in dark theme (Repo-C `ColorExamplesScreen` renders `ErrorContainer` + `InverseSurface` side-by-side as smoke test).
- Respect `fontScale` via `LocalDensity` (Repo-E `CatalogTheme` provides `Density(fontScale)` — honor it, don't hard-code `fontSize`).

## Motion & reduced motion

- Wavy indicators are indeterminate — ensure alternative text `contentDescription = "Loading"` on container. No motion should be required to perceive state; always pair with label.

## Checklist

- [ ] All `IconButton` have real `contentDescription` + `TooltipBox` (or at minimum `contentDescription`).
- [ ] Touch targets ≥48dp (verify via Layout Inspector).
- [ ] `semantics` on toggled/expanded controls.
- [ ] Focus order: toolbar/FAB before list, `zIndex` applied.
- [ ] Type scale from theme, `fontScale` respected.
- [ ] Dark/light contrast verified (especially `onPrimaryContainer`, `inverseSurface`).

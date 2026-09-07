# Layouts — Expressive + Adaptive

## Expressive layout intent

- One hero, calm secondaries, intentional asymmetry. Example: Repo-C `HomeScreen.kt` uses `LazyVerticalGrid(GridCells.Fixed(2))` with `Card(surfaceContainer, RoundedCornerShape(16.dp), aspectRatio 4/3)` — dashboard is more expressive than Repo-E's plain `LazyColumn`. Use grid for browsing, list for scanning.
- Spacious padding (16–24dp) around hero; tighter (12/8dp) in dense sections.

## Adaptive — phone → tablet → foldable → desktop

### Window size class

```kotlin
val info = currentWindowAdaptiveInfo() // androidx.compose.material3.adaptive
val maxActions = if (info.windowSizeClass.minWidth >= 600.dp) 5 else 3
AppBarRow(overflowIndicator = { /* MoreVert */ }, maxItemCount = maxActions) { ... }
```
Ref: `emertozd/AppBarSamples.kt: WithAdaptiveActions`.

### Navigation adaptation

- Phone (<600dp): `NavigationBar`
- Tablet (600–840dp): `NavigationRail` + `WideNavigationRail`
- Expanded (>840dp): `WideNavigationRail` or `NavigationSuiteScaffold` (auto-switches bar↔rail). Ref: `emertozd/NavigationSuiteScaffoldSamples.kt`.
- Foldable: `ThreePaneScaffold` (list-detail-extra). Ref: `emertozd/ThreePaneScaffoldSample.kt`.

### Pane scaffolds

```kotlin
// List-detail for notes app
NavigationSuiteScaffold(
    navigationSuiteItems = { /* items */ },
    layoutType = SuiteScaffoldLayoutType.calculateFromAdaptiveInfo(info)
) {
    ListDetailPaneScaffold(
        directive = PaneScaffoldDirective.Default,
        value = PaneScaffoldValue(...)
    ) { /* panes */ }
}
```

### Floating toolbar placement (critical)

- Compose toolbar **before** `LazyColumn` and apply `Modifier.zIndex(1f)` so it stays on top and receives focus first. Repo-E mandates this; Repo-C places after (a11y order reversed — anti-pattern). Ref: `emertozd/FloatingToolbarSamples.kt`.
- Use `FloatingToolbarDefaults.ScreenOffset` for edge inset, `exitAlwaysScrollBehavior(Bottom/End)` + `nestedScroll` for collapse.

### Responsive FAB / AppBar

- Collapse: `val collapsed by remember { derivedStateOf { listState.firstVisibleItemIndex > 0 } }` → `LargeExtendedFloatingActionButton(expanded = !collapsed)` (Repo-C `LargeFabVariant2.kt`). Do not observe raw `firstVisibleItemScrollOffset` every pixel.

### Spacing tokens

- `ScreenOffset` (toolbar), `ConnectedSpaceBetween` (button group), `SmallContentPadding` (buttons). Avoid hard `64.dp` / `56.dp` where token exists (`LargeIconSize`).

## Edge-to-edge

- Call `enableEdgeToEdge()` in `Activity`. Use `Scaffold(contentWindowInsets = WindowInsets.safeDrawing)` and `consumeWindowInsets`. See `edge-to-edge` skill for IME + nav bar handling. Expressive toolbars should `align(BottomCenter)` with `ScreenOffset` to avoid gesture nav overlap.

## Checklist for tablet adaptation

- [ ] `currentWindowAdaptiveInfo()` drives `maxItemCount`, grid `columns`, pane count.
- [ ] Nav switches bar→rail→wide rail, not always bottom bar.
- [ ] Toolbar/FAB collapse on scroll via `derivedStateOf`.
- [ ] Content uses `surfaceContainer*` hierarchy so panes read as distinct depths.

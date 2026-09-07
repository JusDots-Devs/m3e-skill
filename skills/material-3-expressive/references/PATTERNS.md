# Patterns — Production Recipes

## Notes screen (Test A)

- **Structure**: `Scaffold(topBar=CenterAlignedTopAppBar(title="Notes", actions=AppBarRow(search, more)), floatingActionButton=LargeExtendedFloatingActionButton(Add+ "New note", expanded=!collapsed))` + `LazyVerticalGrid(Fixed(2) on phone, Fixed(3) on tablet via adaptive)` of `ElevatedCard(surfaceContainer, 16.dp, onClick)` with title/body/date. Hero = single FAB, not card decoration. Ref: `meticha/GridTile.kt` card pattern + `emertozd/AppBarSamples.kt` adaptive actions.
- **Expressive touch**: `shapes()` on FAB only; cards static to keep scannability when count >20. Wavy only for sync loading.

## Settings screen (Test B)

- **Restraint.** Settings must be scannable → minimal expressive.
- Use `SegmentedListItems` (outer 28dp / inner 6dp, `surfaceContainerLow`, 2dp gaps) grouping related toggles. Each `ListItem` with `Switch`/`Checkbox`. No wavy, no vibrant toolbar, no large FAB. Ref: `emertozd/ListSamples.kt` + `segmented-connect-list` skill.
- Top: `TopAppBar` (not flexible large) to preserve density.

## Music player (Test D)

- Hero = `LargeExtendedFloatingActionButton(Play/Pause, LargeIconSize)` + `CircularWavyProgressIndicator` for buffering.
- Controls: `ConnectedButtonGroup` (Prev/Play/Next) + `Slider(state=rememberSliderState())` for seek + `HorizontalFloatingToolbar(expanded, vibrantColors)` for queue/shuffle/repeat over album art. Ref: `emertozd/FloatingToolbarSamples.kt` vibrant.

## Notes → tablet adaptation (Test E)

- Phone: single pane `LazyVerticalGrid(Fixed(2))`.
- Tablet: `ListDetailPaneScaffold` (list 360dp + detail flex) + `NavigationSuiteScaffold` (rail). Drive via `currentWindowAdaptiveInfo().windowSizeClass`. FAB stays on list pane only. No duplication of FAB in detail.

## M2 → M3 Expressive migration (Test F)

1. Replace `MaterialTheme` → `MaterialExpressiveTheme` with 4-way colorScheme (see `THEMING.md`), keep `MaterialTheme` fallback.
2. Buttons: add `shapes=ButtonDefaults.shapes()` / `IconButtonDefaults.shapes()`; replace manual `RoundedCornerShape(50)` with tokens.
3. Surfaces: map `Card(elevation=4.dp)` → `ElevatedCard` or `Card(containerColor=surfaceContainer)`.
4. AppBar: `TopAppBar(scrollBehavior=enterAlwaysScrollBehavior())` + `nestedScroll`.
5. Loading: `CircularProgressIndicator` → `CircularWavyProgressIndicator` where hero.
6. Verify `contentDescription`, tooltips, `WindowInsets.safeDrawing`.

## Empty / loading / error states

- **Empty**: `Column(center) { Icon(Outlined.Inbox, 48.dp, tint=onSurfaceVariant); Text("No notes yet", headlineSmall); Text("Tap + to create", bodyMedium, onSurfaceVariant); Button(shapes, "Create note") }`
- **Loading (hero)**: `ContainedLoadingIndicator` centered + `LinearWavyProgressIndicator` at top.
- **Error**: `Card(errorContainer, onErrorContainer) { Text(error) }` + `Button("Retry")` (Repo-C `ColorExamplesScreen` error card).

## Navigation pattern (production)

- Stay on `navigation-compose 2.9.7` (`NavHost` + `rememberNavController`) until Navigation3 beta. Repo-C Navigation3 (`NavDisplay` + `NavBackStack` + `@Serializable NavKey`) is demo-only with SNAPSHOT deps — snapshot `nav3Material 1.0.0-SNAPSHOT` breaks reproducibility. When adopting Nav3, keep per-feature `EntryProviderBuilder` extensions (Repo-C pattern) to keep `NavGraph` small.

## Theming smoke screen

- Copy Repo-C `ColorExamplesScreen` (Buttons row + Cards row + ErrorContainer + InverseSurface snackbar mock + shape variants Rounded/Default/Circle) as a debug screen. Add toggle for `dynamicColor` and `expressive` to verify all roles in light/dark.

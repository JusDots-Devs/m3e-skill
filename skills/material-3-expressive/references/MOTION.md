# Motion — Material 3 Expressive

## Principle

Motion communicates state change. Every animation must answer "what changed?" — press morph = activation, wavy = progress, chevron rotation = expand, FAB icon morph = menu open. No decorative looping except hero loading/splash.

## Press shape morph (primary expressive cue)

```kotlin
Button(onClick = {}, shapes = ButtonDefaults.shapes()) { Text("Action") }
FilledIconButton(onClick = {}, shapes = IconButtonDefaults.shapes()) { Icon(...) }
FilledIconToggleButton(checked, shapes = IconButtonDefaults.toggleableShapes()) { Icon(...) }
```
`shapes()` returns `MorphShape` that animates `CircleShape ↔ RoundedCornerShape` on press via `ProgressAnimationSpec`. Apply to **all** buttons/iconButtons for cheapest expressive win. Verified: `meticha/ButtonComposables.kt`, `emertozd/ButtonSamples.kt`.

## Chevron / expand rotation

```kotlin
val rotation by animateFloatAsState(if (expanded) 180f else 0f, label = "chevron")
Icon(KeyboardArrowDown, Modifier.graphicsLayer(rotationZ = rotation))
```
Add `semantics { stateDescription = if(expanded) "Expanded" else "Collapsed" }`. Ref: `meticha/SplitButtonVariant1.kt`.

## Wavy progress

```kotlin
val progress by animateFloatAsState(target, ProgressIndicatorDefaults.ProgressAnimationSpec, label="progress")
CircularWavyProgressIndicator(progress = { progress })
LinearWavyProgressIndicator(progress = { progress })
// Thick hero variant
CircularWavyProgressIndicator(
    modifier = Modifier.size(52.dp),
    stroke = Stroke(width = with(LocalDensity.current){8.dp.toPx()}, cap = StrokeCap.Round),
    progress = { 0.9f }
)
ContainedLoadingIndicator(progress = { progress }) // determinate
LoadingIndicator() // indeterminate
```
Spring alternative: `spring(dampingRatio = Spring.DampingRatioNoBouncy, stiffness = Spring.StiffnessVeryLow, visibilityThreshold = 1/1000f)`. Ref: `meticha/ProgressIndicatorComposables.kt`, `emertozd/ProgressIndicatorSamples.kt`.

## FAB menu icon morph

```kotlin
val checkedProgress by ToggleFloatingActionButtonDefaults.animateIcon(progress)
val painter = rememberVectorPainter(
    default = Icons.Filled.Add, // or rememberVectorPainter
    // morph to Close when checkedProgress > 0.5
)
ToggleFloatingActionButton(checkedProgress = { checkedProgress }) {
    Icon(painter, contentDescription = null)
}
```
Plus `animateFloatingActionButton(visible = visible || expanded)` and `BackHandler(enabled = expanded) { expanded = false }`. Ref: `meticha/FabMenuComposable.kt`.

## Pull-to-refresh scale

```kotlin
val scale = LinearOutSlowInEasing.transform(state.distanceFraction).coerceIn(0f, 1f)
PullToRefreshDefaults.LoadingIndicator(
    state, isRefreshing,
    Modifier.align(TopCenter).graphicsLayer(scaleX = scale, scaleY = scale)
)
```
Wrap content in `Modifier.pullToRefresh(state, isRefreshing, onRefresh)`. Provide `IconButton(Refresh)` alternative. Ref: `meticha/RefreshIndicatorComposable.kt`.

## Scroll-driven collapse

```kotlin
val collapsed by remember { derivedStateOf { listState.firstVisibleItemIndex > 0 } }
LargeExtendedFloatingActionButton(expanded = !collapsed) { ... }
HorizontalFloatingToolbar(expanded = !collapsed, floatingToolbarState = rememberFloatingToolbarState()) { ... }
```
Use `derivedStateOf` — not raw offset — to avoid per-pixel recomposition.

## Specs to reuse

- `ProgressIndicatorDefaults.ProgressAnimationSpec` for progress.
- `LinearOutSlowInEasing` for pull distance → scale.
- `Spring.DampingRatioNoBouncy` + `StiffnessVeryLow` for gentle wavy spring (Repo-C).
- Label all `animate*AsState` with `label=` for tooling.

## When NOT to animate

- Dense settings/data tables — keep static shapes.
- Background ornaments (particle splash `meticha/SplashScreen.kt` is demo-only anti-pattern — recreates `Random` each recomposition, heavy `Canvas`).
- Never animate >1 hero at once on same screen.


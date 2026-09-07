# Components — Catalog & Selection Rules

> All APIs are `androidx.compose.material3.*` under `@OptIn(ExperimentalMaterial3ExpressiveApi::class)`. Verified in repos above.

## Buttons & Toggle Buttons

| Component | API | When to use |
|---|---|---|
| Button | `Button(shapes=ButtonDefaults.shapes())` | Primary action (one per screen) |
| Elevated | `ElevatedButton(shapes=...)` | On `surfaceContainer`, needs lift |
| FilledTonal | `FilledTonalButton(shapes=...)` | Secondary, lower emphasis |
| Outlined/Text | `OutlinedButton` / `TextButton` | Tertiary / inline |
| IconButton | `IconButton / FilledIconButton / FilledTonalIconButton / OutlinedIconButton` + `IconButtonDefaults.shapes()` | Toolbar / row actions |
| Toggle | `ToggleButton`, `ElevatedToggleButton`, `FilledIconToggleButton`, `FilledTonalIconToggleButton` + `IconButtonDefaults.toggleableShapes()` | Selectable icon; morph via `checked` |
| Size tokens | `ButtonDefaults.heightIn(size)`, `contentPaddingFor(size)`, `iconSizeFor(size)`, `textStyleFor(size)`, `iconSpacingFor(size)` | `ExtraSmall/Medium/Large/ExtraLargeContainerHeight`; use instead of hard dp |

Refs: `meticha/ButtonComposables.kt`, `emertozd/ButtonSamples.kt`, `emertozd/IconButtonSamples.kt`, `emertozd/ToggleButtonSamples.kt`.

## Button Groups

- `ButtonGroup(overflowIndicator={FilledIconButton(MoreVert) { menuState.show() }}) { clickableItem("1") ... }` — icon/text rows with overflow.
- `ConnectedButtonGroup` — `ButtonGroupDefaults.connectedLeadingButtonShapes() / connectedMiddle / connectedTrailing`, `ConnectedSpaceBetween` spacing, `Row(weight 1f/1.5f/1f)` for weighted segments. Add `Modifier.semantics{role=Role.RadioButton}.selectableGroup()` for single-select.
- Ref: `meticha/ConnectedButtonGroupComposable.kt`.

## Split Button

- `SplitButtonLayout(leadingButton={SplitButtonDefaults.LeadingButton(onClick){Text("Edit")}}, trailingButton={SplitButtonDefaults.TrailingButton(onClick){Icon(KeyboardArrowDown)}})` — use `animateFloatAsState(if(expanded)180f else 0f)` → `graphicsLayer(rotationZ)` on trailing icon, `semantics{stateDescription}`. Variant `OutlinedLeading/TrailingButton` for outlined style. Size overload: `leadingButtonShapesFor(size)` etc. Ref: `meticha/SplitButtonVariant3.kt`, `emertozd/SplitButtonSamples.kt`.

## FABs

- `FloatingActionButton`, `ExtendedFloatingActionButton`, `LargeExtendedFloatingActionButton(icon={Icon(Add, LargeIconSize)}, text={Text("...")})`.
- **FAB Menu**: `FloatingActionButtonMenu(expanded, button={ToggleFloatingActionButton(checkedProgress->{animateIcon}) {Icon(rememberVectorPainter(Add→Close))}}) { FloatingActionButtonMenuItem(...) }` + `animateFloatingActionButton(visible=derivedStateOf(listState.firstVisibleItemIndex==0).value || expanded)`, `BackHandler`, `traversalIndex/toggleable` a11y. Ref: `meticha/FabMenuComposable.kt`, `emertozd/FloatingActionButtonMenuSamples.kt`.
- Collapse rule: `derivedStateOf { listState.firstVisibleItemIndex==0 }` controls `expanded`/`visible` — do not observe raw scroll offset.

## Toolbars

- `HorizontalFloatingToolbar(expanded, floatingToolbarState, colors=vibrantFloatingToolbarColors(), expandedShadowElevation, leadingContent, trailingContent=AppBarRow(overflowIndicator), content={FilledIconButton(Modifier.size(64.dp))})` — place with `Modifier.align(BottomCenter).offset(y=-FloatingToolbarDefaults.ScreenOffset)`, `zIndex(1f)` and compose **before** `LazyColumn`. Scroll collapse: `FloatingToolbarDefaults.exitAlwaysScrollBehavior(ExitDirection.Bottom)` + `Modifier.nestedScroll(behavior)`. Vertical variant analogous with `AppBarColumn`, `CenterEnd`, `floatingToolbarVerticalNestedScroll`. Ref: `meticha/FloatingToolBarVariant2.kt`, `emertozd/FloatingToolbarSamples.kt` (9 samples incl. vibrant).

## App Bars

- Simple: `TopAppBar(title, navigationIcon, actions=AppBarRow{...})`
- Expressive adaptive: `CenterAlignedTopAppBar`, `MediumFlexibleTopAppBar`, `LargeFlexibleTopAppBar`, `TwoRowsTopAppBar`, `TopAppBarDefaults.(pinned/enterAlways/exitUntilCollapsed)ScrollBehavior` + `Modifier.nestedScroll`. Adaptive actions: `val maxItemCount = if(currentWindowAdaptiveInfo().windowSizeClass.minWidth>=600.dp) 5 else 3`; pass to `AppBarRow(maxItemCount)`. Ref: `emertozd/AppBarSamples.kt` (15 variants).

## Bottom App Bar

- `FlexibleBottomAppBar(contentPadding=96.dp, arrangement=FlexibleFixedHorizontalArrangement, scrollBehavior=BottomAppBarDefaults.exitAlwaysScrollBehavior())` with `AppBarRow` + optional centered `FilledIconButton(56.dp)` or docked FAB. Variant via `Arrangement.SpaceBetween/Center`. Ref: `meticha/BottomAppBarVariant2.kt`.

## Navigation

- `NavigationBar` (phone), `NavigationRail` + `WideNavigationRail` / `ModalWideNavigationRail(state=rememberWideNavigationRailState(), collapsedContent/expandedContent)` (tablet/foldable). Items: `WideNavigationRailItem(icon, label, selected)`. Control via `scope.launch{ state.expand()/collapse() }`. For adaptive hosting use `NavigationSuiteScaffold` or `ThreePaneScaffold` (Repo-E `NavigationSuiteScaffoldSamples.kt`, `ThreePaneScaffoldSample.kt`). Note: `WideNavigationRail` is experimental — gate with `ExperimentalMaterial3ExpressiveApi`.

## Cards & Surfaces

- `Card`, `ElevatedCard`, `OutlinedCard` with `CardDefaults.cardColors(containerColor=surfaceContainer)`. `Surface(onClick, tonalElevation)` for selectable rows. Repo-C `GridTile` pattern: `Card(RoundedCornerShape(16.dp), surfaceContainer) { Box(aspectRatio 4/3) { Icon + Text(labelMedium) } }`.

## Chips

- `AssistChip`, `FilterChip`, `InputChip`, `SuggestionChip` with corner-morph on select. Ref: `emertozd/ChipSamples.kt`.

## Selection & Toggle

- `Checkbox`, `RadioButton`, `Switch`, `Slider(state=rememberSliderState())` / `RangeSlider`, `SegmentedButton` / `ToggleButton` group, `SwipeToDismiss`. Note slider breaking change: Repo-C legacy `Slider(value, onValueChange)` vs M3 1.5 `Slider(state=rememberSliderState())` — use new API.

## Progress & Loading

- `CircularWavyProgressIndicator(progress={0.9f})`, `LinearWavyProgressIndicator`, `ContainedLoadingIndicator` (determinate/indeterminate), `LoadingIndicator`. Thick variant: `Stroke(width=LocalDensity.toPx(8.dp), cap=Round)` + `Modifier.size(52.dp)`. Animation: `animateFloatAsState(target, animationSpec=ProgressIndicatorDefaults.ProgressAnimationSpec)` or `spring(DampingRatioNoBouncy, StiffnessVeryLow)`. Ref: `meticha/ProgressIndicatorComposables.kt`.

## Pull-to-Refresh

- `val state=rememberPullToRefreshState(); Box(Modifier.pullToRefresh(state,isRefreshing,onRefresh)) { LazyColumn; PullToRefreshDefaults.LoadingIndicator(state,isRefreshing, Modifier.align(TopCenter).graphicsLayer(scaleX/scaleY=LinearOutSlowInEasing.transform(state.distanceFraction).coerceIn(0,1))) }`. Add `IconButton(Refresh)` alternative for a11y. Ref: `meticha/RefreshIndicatorComposable.kt`.

## Lists

- `ListItem(headlineContent, supportingContent, leadingContent, trailingContent)`, `HorizontalDivider`, `SegmentedListItems` (connected 28/6 shape — see `segmented-connect-list` skill). Ref: `emertozd/ListSamples.kt`.

## Text Fields & Search

- `TextField` / `OutlinedTextField` with `rememberTextFieldState()`, `TextFieldLineLimits.SingleLine`, icons, placeholder, prefix/suffix, error. `SearchBar` / `DockedSearchBar`. Ref: `emertozd/TextFieldSamples.kt` (14 samples), `emertozd/SearchBarSamples.kt`.

## Dialogs, Sheets, Menus, Tooltips

- `AlertDialog`, `ModalBottomSheet(rememberBottomSheetState)`, `DropdownMenu` + `DropdownMenuItem`, `ExposedDropdownMenu`, `TooltipBox(state=rememberTooltipState(), positionProvider=TooltipDefaults.rememberTooltipPositionProvider(), tooltip={PlainTooltip{Text(...)}}) { IconButton... }`, `Badge/BadgedBox`, `SnackbarHost`. Ref: `emertozd/DialogSamples.kt`, `emertozd/BottomSheetSamples.kt`, `emertozd/MenuSamples.kt`, `emertozd/TooltipSamples.kt`.

## Tabs, Segmented Controls, Progress Variants

- `TabRow / PrimaryTabRow / SecondaryTabRow`, `SegmentedButton` with size tokens, `LinearProgressIndicator` (legacy) vs wavy. Ref: `emertozd/TabSamples.kt`, `emertozd/SegmentedButtonSamples.kt`.

## Carousel, Date/Time Pickers, Scrollbar

- `Carousel`, `DatePicker`, `TimePicker`, `Scrollbar` (LazyColumn), `SwipeToDismiss`, `Surface` selectable. Ref: `emertozd/CarouselSamples.kt`, `emertozd/DatePickerSamples.kt`, `emertozd/TimePickerSamples.kt`.

## Component decision rules (quick)

- Need single primary action → `LargeExtendedFloatingActionButton` (hero) or `Button(shapes)`.
- Need 3–5 related actions → `ButtonGroup` / `ConnectedButtonGroup` (not separate buttons).
- Need split primary+overflow → `SplitButtonLayout`.
- Need contextual edit/format actions over content → `HorizontalFloatingToolbar`.
- Need nav on large screen → `WideNavigationRail` / `NavigationSuiteScaffold`.
- Need loading over content → `PullToRefresh` + wavy; need standalone loading → `ContainedLoadingIndicator`.

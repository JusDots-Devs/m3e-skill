# Compose Implementation — Exact APIs & Snippets

> Gradle reference: M3 `1.5.0-alpha26` (Repo-E) / BOM `2026.08.00` (Repo-E) vs `2025.06.02` (Repo-C), Compose UI `1.13.0-alpha01`, Adaptive `1.3.0`, `compileSdk 36–37`, `minSdk 24–31`, `ExperimentalMaterial3ExpressiveApi` globally or per-file.

## Opt-in

```kotlin
// per-file (preferred scope)
@OptIn(ExperimentalMaterial3ExpressiveApi::class)
@Composable fun MyScreen() { ... }

// or global (Repo-C build.gradle.kts)
kotlinOptions { freeCompilerArgs += "-Xopt-in=androidx.compose.material3.ExperimentalMaterial3ExpressiveApi" }
```

## Dependencies (production minimal)

```kotlin
implementation("androidx.compose.material3:material3:1.5.0-alpha26")
implementation("androidx.compose.material:material-icons-extended")
implementation("androidx.compose.material3:material3-adaptive-navigation-suite:1.3.0")
implementation("androidx.compose.ui:ui:1.13.0-alpha01")
```

## Button (expressive morph)

```kotlin
@OptIn(ExperimentalMaterial3ExpressiveApi::class)
@Composable fun ExpressiveButtons() {
    var checked by remember { mutableStateOf(false) }
    ElevatedButton(onClick = {}, shapes = ButtonDefaults.shapes()) { Text("Elevated") }
    ElevatedToggleButton(checked = checked, onCheckedChange = { checked = it }) { Text("$checked") }
    FilledIconButton(onClick = {}, shapes = IconButtonDefaults.shapes()) {
        Icon(if (checked) Icons.Filled.Lock else Icons.Outlined.Lock, null)
    }
    FilledIconToggleButton(checked = checked, onCheckedChange = { checked = it },
        shapes = IconButtonDefaults.toggleableShapes()) { Icon(Icons.Filled.Lock, null) }
    // Size tokens
    Button(onClick = {}, contentPadding = ButtonDefaults.contentPaddingFor(ButtonDefaults.MediumContainerHeight),
           shapes = ButtonDefaults.shapes()) { Text("Medium", style = ButtonDefaults.textStyleFor(ButtonDefaults.MediumContainerHeight)) }
}
```

## ButtonGroup / ConnectedButtonGroup

```kotlin
@OptIn(ExperimentalMaterial3ExpressiveApi::class)
@Composable fun Groups() {
    val menuState = remember { /* AppBarRow overflow state */ }
    ButtonGroup(overflowIndicator = { FilledIconButton(onClick = { menuState.show() }) { Icon(Icons.Default.MoreVert, null) } }) {
        repeat(6) { clickableItem(onClick = {}, label = "$it") }
    }
    // Connected (single-select)
    var selected by remember { mutableStateOf(0) }
    Row(Modifier.selectableGroup().padding(8.dp), horizontalArrangement = Arrangement.spacedBy(ButtonGroupDefaults.ConnectedSpaceBetween)) {
        ToggleButton(checked = selected==0, onCheckedChange={selected=0},
            shapes = ButtonGroupDefaults.connectedLeadingButtonShapes()) { Icon(Icons.Default.Work,null); Spacer(Modifier.width(ToggleButtonDefaults.IconSpacing)); Text("Work") }
        ToggleButton(checked = selected==1, onCheckedChange={selected=1},
            shapes = ButtonGroupDefaults.connectedMiddleButtonShapes(), modifier = Modifier.weight(1.5f)) { Text("Restaurant") }
        ToggleButton(checked = selected==2, onCheckedChange={selected=2},
            shapes = ButtonGroupDefaults.connectedTrailingButtonShapes()) { Text("Coffee") }
    }
}
```

## SplitButton

```kotlin
@OptIn(ExperimentalMaterial3ExpressiveApi::class)
@Composable fun Split() {
    var expanded by remember { mutableStateOf(false) }
    val rot by animateFloatAsState(if(expanded)180f else 0f, label="rot")
    SplitButtonLayout(
        leadingButton = { SplitButtonDefaults.LeadingButton(onClick={}) { Icon(Icons.Default.Edit,null); Spacer(Modifier.width(ButtonDefaults.IconSpacing)); Text("Edit") } },
        trailingButton = { SplitButtonDefaults.TrailingButton(onClick={expanded=!expanded}) {
            Icon(Icons.Default.KeyboardArrowDown, null, Modifier.graphicsLayer(rotationZ=rot).semantics{stateDescription=if(expanded)"Expanded" else "Collapsed"})
        } }
    )
    DropdownMenu(expanded, onDismissRequest={expanded=false}) {
        DropdownMenuItem(text={Text("Send")}, onClick={}, leadingIcon={Icon(Icons.Default.Email,null)})
        HorizontalDivider(); DropdownMenuItem(text={Text("Settings")}, onClick={}, leadingIcon={Icon(Icons.Default.Settings,null)})
    }
}
```

## FAB Menu

```kotlin
@OptIn(ExperimentalMaterial3ExpressiveApi::class)
@Composable fun FabMenu(listState: LazyListState) {
    var expanded by rememberSaveable { mutableStateOf(false) }
    val visible by remember { derivedStateOf { listState.firstVisibleItemIndex==0 } }
    val fabVisible = visible || expanded
    BackHandler(enabled=expanded){ expanded=false }
    Box(Modifier.fillMaxSize().semantics{isTraversalGroup=true}) {
        LazyColumn(state=listState) { items(100){ Text("Item $it", Modifier.padding(16.dp)) } }
        FloatingActionButtonMenu(
            expanded=expanded,
            button={
                ToggleFloatingActionButton(
                    checked=expanded, onCheckedChange={expanded=it},
                    modifier=Modifier.animateFloatingActionButton(visible={fabVisible}, alignment=Alignment.BottomEnd)
                ){
                    val p by ToggleFloatingActionButtonDefaults.animateIcon(if(expanded) 1f else 0f)
                    Icon(rememberVectorPainter(Icons.Filled.Add), null) // morph to Close via p
                }
            }
        ){
            FloatingActionButtonMenuItem(onClick={}, text={Text("Reply")}, icon={Icon(Icons.Default.Reply,null)})
            FloatingActionButtonMenuItem(onClick={}, text={Text("Archive")}, icon={Icon(Icons.Default.Archive,null)})
        }
    }
}
```

## Floating Toolbar

```kotlin
@OptIn(ExperimentalMaterial3ExpressiveApi::class)
@Composable fun Toolbars(listState: LazyListState) {
    val behavior = FloatingToolbarDefaults.exitAlwaysScrollBehavior(ExitDirection.Bottom)
    Box(Modifier.fillMaxSize().nestedScroll(behavior)) {
        // Toolbar FIRST + zIndex
        HorizontalFloatingToolbar(
            expanded=true, modifier=Modifier.align(Alignment.BottomCenter).offset(y=-FloatingToolbarDefaults.ScreenOffset).zIndex(1f),
            floatingToolbarState=rememberFloatingToolbarState(), scrollBehavior=behavior,
            colors=FloatingToolbarDefaults.vibrantFloatingToolbarColors(),
            content={ FilledIconButton(onClick={}, modifier=Modifier.size(64.dp)){ Icon(Icons.Default.Add,null) } },
            trailingContent={ AppBarRow(overflowIndicator={IconButton(onClick={}){Icon(Icons.Default.MoreVert,null)}}){
                clickableItem(onClick={}, icon={Icon(Icons.Default.Search,null)}, label="Search")
                clickableItem(onClick={}, icon={Icon(Icons.Default.Edit,null)}, label="Edit")
            }}
        )
        LazyColumn(state=listState, contentPadding=PaddingValues(bottom=100.dp)) { items(75){ Text("Row $it", Modifier.padding(16.dp)) } }
    }
}
```

## Progress (wavy + pull)

```kotlin
@OptIn(ExperimentalMaterial3ExpressiveApi::class)
@Composable fun Progress() {
    var v by remember { mutableStateOf(0.6f) }
    val p by animateFloatAsState(v, ProgressIndicatorDefaults.ProgressAnimationSpec, label="p")
    CircularWavyProgressIndicator(progress={p}, modifier=Modifier.size(52.dp),
        stroke=Stroke(width=with(LocalDensity.current){8.dp.toPx()}, cap=StrokeCap.Round))
    LinearWavyProgressIndicator(progress={p}, modifier=Modifier.height(14.dp))
    ContainedLoadingIndicator(progress={p}); LoadingIndicator()
    Slider(state=rememberSliderState(value=v, onValueChange={v=it}, valueRange=0f..1f))
    // Pull
    val ps=rememberPullToRefreshState(); var refreshing by remember{mutableStateOf(false)}
    Box(Modifier.pullToRefresh(ps,refreshing,{refreshing=true})){
        LazyColumn{ items(50){ Text("Item $it") } }
        val s=LinearOutSlowInEasing.transform(ps.distanceFraction).coerceIn(0f,1f)
        PullToRefreshDefaults.LoadingIndicator(ps,refreshing,Modifier.align(Alignment.TopCenter).graphicsLayer(scaleX=s,scaleY=s))
    }
}
```

## AppBar (adaptive)

```kotlin
@OptIn(ExperimentalMaterial3ExpressiveApi::class)
@Composable fun AppBars() {
    val max = if(currentWindowAdaptiveInfo().windowSizeClass.minWidth>=600.dp) 5 else 3
    Scaffold(topBar={
        CenterAlignedTopAppBar(title={Text("Notes")},
            navigationIcon={IconButton(onClick={}){Icon(Icons.Default.Menu,null)}},
            actions={ AppBarRow(maxItemCount=max, overflowIndicator={IconButton(onClick={}){Icon(Icons.Default.MoreVert,null)}}){
                clickableItem(onClick={}, icon={Icon(Icons.Default.Search,null)}, label="Search")
            }},
            scrollBehavior=TopAppBarDefaults.pinnedScrollBehavior()
        )
    }, modifier=Modifier.nestedScroll(TopAppBarDefaults.pinnedScrollBehavior().nestedScrollConnection)) { pad ->
        Box(Modifier.padding(pad))
    }
}
```

## Navigation Rail (wide)

```kotlin
@OptIn(ExperimentalMaterial3ExpressiveApi::class)
@Composable fun WideRail() {
    val state=rememberWideNavigationRailState(); val scope=rememberCoroutineScope()
    Row{
        WideNavigationRail(state=state,
            header={ IconButton(onClick={scope.launch{if(state.currentValue==WideNavigationRailValue.Expanded) state.collapse() else state.expand()}}){Icon(Icons.Default.MenuOpen,null)} },
            content={
                WideNavigationRailItem(icon={Icon(Icons.Filled.Home,null)}, label={Text("Home")}, selected=true, onClick={})
                WideNavigationRailItem(icon={Icon(Icons.Outlined.Star,null)}, label={Text("Starred")}, selected=false, onClick={})
            }
        )
        Box(Modifier.weight(1f).fillMaxSize(), contentAlignment=Alignment.Center){ Text("Content") }
    }
}
```

## Tracing

- Button morph: `meticha/ButtonComposables.kt:10`, `emertozd/ButtonSamples.kt`
- Groups: `meticha/ConnectedButtonGroupComposable.kt:22`, `emertozd/ButtonGroupSamples.kt`
- Split: `meticha/SplitButtonVariant3.kt`
- FAB menu: `meticha/FabMenuComposable.kt:35`
- Toolbar: `meticha/FloatingToolBarVariant2.kt`, `emertozd/FloatingToolbarSamples.kt:9samples`
- Progress: `meticha/ProgressIndicatorComposables.kt`, `emertozd/ProgressIndicatorSamples.kt`
- Pull: `meticha/RefreshIndicatorComposable.kt`
- AppBar adaptive: `emertozd/AppBarSamples.kt: WithAdaptiveActions`
- Rail: `meticha/WideNavigationRail.kt`

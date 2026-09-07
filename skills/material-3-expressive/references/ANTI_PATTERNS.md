# Anti-Patterns — What to Avoid

> Each item cites the fragile source.

## Theming

- **Unreachable branch** — `when { isDarkTheme -> dark; !isDarkTheme -> light; else -> expressive }` (Repo-C `Theme.kt`). `else` never executes. Never copy `when` with `isDark / !isDark` covering all cases.
- **Always-expressive wrapper** — Repo-C wraps in `MaterialExpressiveTheme` even when baseline intended. Keep switchable `if(expressive) MaterialExpressiveTheme else MaterialTheme` (Repo-E `CatalogTheme`).
- **Missing status bar SideEffect** — Repo-C omits `WindowCompat.getInsetsController` → dark status icons invisible. Always set `isAppearanceLightStatusBars`.

## Dimensions & tokens

- **Hard-coded sizes**: `64.dp` FAB, `56.dp` FilledIconButton, `52.dp` progress in Repo-C. Use `FloatingActionButtonDefaults.LargeIconSize`, `ButtonDefaults.*ContainerHeight`, `LocalDensity.toPx(8.dp)` tokens.
- **Arbitrary radii** like `13.dp`. Use `4/12/16/28.dp` or `CircleShape`.

## Navigation & imports

- **Snapshot Navigation3 in prod**: `androidx.navigation3 1.0.0-SNAPSHOT` + `lifecycle-viewmodel-navigation3 1.0.0-alpha03` (Repo-C) breaks reproducible builds. Stay on `navigation-compose 2.9.7` until beta.
- **Hilt for a catalog scaffold**: `MyApplication @HiltAndroidApp + @AndroidEntryPoint` on `MainActivity` with no injected ViewModel (Repo-C). Overhead with no value.
- **Import pollution**: `android.R.attr.checked` + `android.R.attr.contentDescription` shadowing in `SplitButtonVariant3.kt` — never import `android.R.attr`.
- **Duplicate material artifacts**: `material-icons-extended` + `material-icons-extended-android` both in `implementation` (Repo-E `app/build.gradle.kts`). Duplicate APK bloat; keep one.
- **`ui-test` in implementation**: Repo-E puts `ui-test` under `implementation` not `androidTestImplementation`.

## Accessibility

- **`"Localized description"` placeholders** on every `Icon` (Repo-C `BottomAppBarVariant*`, `SplitButtonVariant1`). Lint failure. Use `stringResource`.
- **Missing TooltipBox**: Repo-C has none; Repo-E wraps every icon. For expressive toolbars/FAB menus tooltips are required on tablet/desktop.
- **Toolbar after content**: Repo-C composes `HorizontalFloatingToolbar` after `LazyColumn` → TalkBack hits list before toolbar, `floatingToolbarVerticalNestedScroll` broken. Compose toolbar first + `zIndex(1f)`.

## Motion & state

- **Particle splash**: `SplashScreen.kt` creates `Random` + `Canvas` particles without `remember` → recreation each recomposition, heavy. Use `AnimatedVisibility` or simple scale; particle is demo anti-pattern.
- **`mutableFloatStateOf` slider on M3 1.5**: Repo-C `Slider(value, onValueChange)` is legacy; M3 1.5 `Slider(state=rememberSliderState())` is current (Repo-E). Using old API will break on bump.
- **Per-pixel scroll observation**: Observing `firstVisibleItemScrollOffset` without `derivedStateOf` causes per-pixel recomposition. Use `derivedStateOf { firstVisibleItemIndex==0 }`.

## Code organization

- **Inconsistent NavGraph signatures**: `VerticalFloatingToolbarNavGraph()` / `WideNavigationRailNavGraph()` take no `NavBackStack` while siblings do → `backStack.removeLastOrNull()` unavailable (Repo-C).
- **1000-line `AppBarSamples.kt`** (Repo-E) duplicates `TooltipBox+PlainTooltip+liveRegion` 20× with `TODO b/496338253`. Extract `TooltipIconButton` helper.
- **Scattered `ExperimentalMaterial3ExpressiveApi` on every file** vs isolating to wrapper — both repos do file-wide opt-in; prefer scoping to theme/button wrappers to reduce blast radius when API graduates.

## General

- **Labeling any rounded/animated UI as "M3 Expressive"** — expressive requires token shapes + `shapes()` morph + at least one wavy/vibrant/large hero, not just `RoundedCornerShape(16.dp)`.
- **Over-expressing dense screens**: applying wavy + vibrant + large FAB on settings/data tables destroys scannability. Restraint rule: ≤1 hero.

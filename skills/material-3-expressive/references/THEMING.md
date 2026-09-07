# Theming — Material 3 Expressive

## Theme wrapper

```kotlin
@OptIn(ExperimentalMaterial3ExpressiveApi::class)
@Composable
fun AppTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    dynamicColor: Boolean = true,
    expressive: Boolean = true,
    content: @Composable () -> Unit
) {
    val context = LocalContext.current
    val colorScheme = when {
        dynamicColor && darkTheme  -> dynamicDarkColorScheme(context)
        dynamicColor               -> dynamicLightColorScheme(context)
        darkTheme && expressive    -> darkColorScheme() // or expressiveDark when available
        expressive                 -> expressiveLightColorScheme()
        darkTheme                  -> darkColorScheme()
        else                       -> lightColorScheme()
    }
    // Keep MaterialExpressiveTheme vs MaterialTheme switchable (Repo-E pattern)
    if (expressive) MaterialExpressiveTheme(colorScheme = colorScheme, content = content)
    else MaterialTheme(colorScheme = colorScheme, content = content)
}
```

**Why:** Repo-C `Theme.kt` has unreachable `else -> expressiveLight` branch — never copy `when { isDark -> dark; !isDark -> light; else -> expressive }`. Use explicit 4-way branch above. Repo-E `CatalogTheme` toggles expressive vs baseline via `ExpressiveThemeMode` + DataStore; keep same ability for A/B testing.

## Color modes (Repo-E `Themes.kt`)

- `ColorMode.Dynamic` → `dynamicLight/DarkColorScheme(context)` (Android 12+)
- `ColorMode.Custom` → hard brand scheme (e.g., `#006E2C LightCustomColorScheme`) — only for branded apps, document as brand exception
- `ColorMode.Baseline` → `expressiveLightColorScheme()` or `lightColorScheme()` depending on `ExpressiveThemeMode`

Always provide light+dark; test both. Persist choice via `DataStore` (`UserPreferencesRepository` pattern) if app needs user toggle.

## Dynamic color notes

- Requires `compileSdk 36+`, `minSdk 31` for full fidelity; graceful fallback to expressive baseline on < S.
- Edge-to-edge: call `enableEdgeToEdge()` in `Activity` and propagate via `SideEffect { WindowCompat.getInsetsController(window, view).isAppearanceLightStatusBars = !darkTheme }` (Repo-E does; Repo-C omits — status icons become invisible in dark).

## Typography theming

```kotlin
val AppTypography = Typography(
    bodyLarge = TextStyle(fontSize = 16.sp, lineHeight = 24.sp, letterSpacing = 0.5.sp)
    // keep rest default; override only measured need
)
```
Pass to `MaterialExpressiveTheme(typography = AppTypography)`. Type scale lives in theme, not per-screen.

## Shapes theming

`Shapes` from `MaterialTheme.shapes` uses `extraSmall/small/medium/large/extraLarge` tokens. Override only if brand requires; otherwise use per-component `shapes()` morph.

## Dark / light testing

- Verify `surfaceContainer*` hierarchy in both themes (Repo-C `ColorExamplesScreen` renders Buttons/Cards/ErrorContainer/InverseSurface side-by-side — copy that screen as a theme smoke test).
- Contrast check: Repo-C `ColorItemDetail` shows `Aa` circle with `toHexString()` — keep similar debug overlay during development.

## References

- Theme switch: `emertozd/library/ui/theme/Theme.kt: CatalogTheme`
- Brand custom: `emertozd/library/ui/theme/Theme.kt: LightCustomColorScheme #006E2C`
- Bug to avoid: `meticha/ui/theme/Theme.kt: Material3ExpressiveCatalogTheme`
- Color demo: `meticha/components/colors/ColorComposables.kt: ColorRolesScreen`

# Модули

**Статус:** черновик · **ADR:** [0002](../ADR/ADR_0002_Engine_UE58.md), [0005](../ADR/ADR_0005_Simple_First.md)

Два рабочих модуля: `TDRuntime` (весь геймплей) и `TDUIRuntime` (UI). Зависимость односторонняя: UI знает геймплей, геймплей не знает UI.

## Структура

```
TD/                         ← корень проекта Unreal (TD.uproject)
  Source/
    TD/                     ← primary module (пустой)
    TDRuntime/
      Public/ Private/
        Game/        GameMode, GameState, PlayerController, Campaign, Save
        Grid/        LevelGrid, GridVolume, Route
        Units/       DefenseUnit, Weapon, Suppression, Tractor, RoadObstacle
        Enemies/     Enemy, RouteFollower, Traits, Wreck
        Combat/      DamageLibrary, ProjectileVisual, SuperStrike
        Waves/       WaveDirector
        Building/    BuildComponent
        Camera/      CameraPawn, CameraConfig, CameraBounds (из GrimProtocol)
        Data/        все DataAsset
        Tags/        FTD_GameplayTags
    TDUIRuntime/
      Public/ Private/
        ViewModels/
        Screens/
        Widgets/
  Content/TD/               ← /Game/TD
  Config/
```

## Зависимости Build.cs

| Модуль | Public | Private |
| --- | --- | --- |
| `TDRuntime` | Core, CoreUObject, Engine, InputCore, EnhancedInput, GameplayTags | — |
| `TDUIRuntime` | Core, CoreUObject, Engine, UMG, Slate, SlateCore, CommonUI, ModelViewViewModel, TDRuntime | CommonInput |
| `TDEditor` | Core, CoreUObject, Engine, UnrealEd, TDRuntime | — |

## Правила

- Новый модуль — только с ADR.
- `TDRuntime` не включает заголовки `TDUIRuntime`. Связь — делегаты и состояние, которое читают ViewModel.
- Тесты — Automation Tests в `Private/Tests/` модуля (флаг `WITH_DEV_AUTOMATION_TESTS`).

## Плагины

CommonUI, ModelViewViewModel, EnhancedInput (встроен), GameplayTags (встроен). Voxel Plugin и прочие плагины GrimProtocol не нужны.

# Камера и ввод — архитектура

**Статус:** черновик · **GDD:** [40_UI_UX](../GDD/40_UI_UX.md) · **ADR:** [0006](../ADR/ADR_0006_Camera_From_GrimProtocol.md)

## Обзор

Камера — перенос `AGP_CameraPawn` из GrimProtocol в `ATD_CameraPawn`: SpringArm + Camera, панорама (клавиши и край экрана), вращение, зум с пределами, ограничение `ATD_CameraBoundsVolume`. Настройки — `UTD_CameraConfigDataAsset`.

## Что меняем при переносе

| Было в GrimProtocol | В TD |
| --- | --- |
| `UVoxelSimpleInvokerComponent` | Удалить |
| Связи с туманом войны, мини-картой, сетью | Удалить |
| Префикс `GP`, категории `GP|Camera` | `TD`, `TD|Camera` |
| Только WASD | WASD + стрелки (одно действие `IA_TD_CameraPan`, две привязки) |
| Вращение только мышью | + `IA_TD_CameraRotateStep` (Q/E, шаг 45°) |
| Границы карты — объём | Объём ставится по размеру `ATD_LevelGrid` + запас из данных уровня |

## Enhanced Input

`IMC_TD_Default`:

| Действие | Привязка |
| --- | --- |
| `IA_TD_CameraPan` (2D) | WASD, стрелки |
| `IA_TD_CameraZoom` (1D) | Колесо |
| `IA_TD_CameraRotate` (1D) + `IA_TD_CameraRotateToggle` | Средняя кнопка + движение мыши |
| `IA_TD_CameraRotateStep` (1D) | Q (−), E (+) |
| `IA_TD_Select` | ЛКМ |
| `IA_TD_Cancel` | ПКМ, Esc |
| `IA_TD_BuildSlot1..9` | 1–9 |
| `IA_TD_BuildRepeatModifier` | Shift |
| `IA_TD_Strike1..5` | Z, X, C, V, B |
| `IA_TD_Pause` | Пробел |
| `IA_TD_SpeedToggle` | Tab |
| `IA_TD_ShowRanges` | Alt |

Переназначение — через пользовательские настройки Enhanced Input.

## Тесты

- Перенести тест дефолтного поворота камеры из GrimProtocol (`GPCameraDefaultYawContractTest`) под TD.
- Ручная: все привязки, край экрана, пределы зума и границы карты.

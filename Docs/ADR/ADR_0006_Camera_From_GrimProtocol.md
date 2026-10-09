# ADR-0006 — Камера переносится из GrimProtocol

## Статус
Принято

## Контекст
Владелец хочет RTS-камеру как в GrimProtocol (GDD/40). В GrimProtocol она готова и проверена: `AGP_CameraPawn` (~800 строк), `UGP_CameraConfigDataAsset`, `AGP_CameraBoundsVolume`.

## Решение
- Переносим три класса в `TDRuntime` как `ATD_CameraPawn`, `UTD_CameraConfigDataAsset`, `ATD_CameraBoundsVolume`.
- Удаляем всё, что связано с Voxel Plugin (`UVoxelSimpleInvokerComponent`), туманом войны, мини-картой и сетью.
- Добавляем перемещение стрелками в набор Enhanced Input.
- Добавляем шаг поворота Q/E на 45°.
- Пределы зума — в конфиге камеры.

## Последствия
### Плюсы
- Готовое поведение: панорама, край экрана, вращение, зум, ограничение картой.
### Минусы
- Нужно аккуратно вычистить зависимости GrimProtocol.

## Ссылки
- [TDD/15_Camera_Input](../TDD/15_Camera_Input.md)
- GrimProtocol `Docs/TDD/11_RTS_Camera.md`

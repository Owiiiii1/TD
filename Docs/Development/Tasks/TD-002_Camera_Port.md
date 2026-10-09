# TD-002 — Перенос RTS-камеры из GrimProtocol

## Статус
Не начато

## Этап
M0 Подготовка

## Зависит от
TD-001

## Цель
Перенести камеру GrimProtocol в TDRuntime, убрать лишние зависимости и добавить стрелки и шаг поворота Q/E.

## Что прочитать перед началом
- `CLAUDE.md`, [Coding_Rules](../Coding_Rules.md), [Workflow](../Workflow.md)
- [TDD/15](../../TDD/15_Camera_Input.md), [ADR-0006](../../ADR/ADR_0006_Camera_From_GrimProtocol.md), [GDD/40](../../GDD/40_UI_UX.md) (раздел «Камера и управление»)
- Исходники GrimProtocol: `GP/Source/GPRuntime/{Public,Private}/Camera/*` в репозитории Owiiiii1/RTS, тест `GPCameraDefaultYawContractTest`

## Объём работ
- `ATD_CameraPawn`, `UTD_CameraConfigDataAsset`, `ATD_CameraBoundsVolume` — перенос с переименованием.
- Удалить Voxel invoker и всё, связанное с FoW, мини-картой, сетью.
- Enhanced Input: `IMC_TD_Default` и камерные действия из TDD/15 (пан WASD + стрелки, зум, вращение мышью, шаг Q/E).
- `ATD_PlayerController` (минимальный: подключает IMC, передаёт ввод камере) и `ATD_GameMode` (минимальный: pawn и controller по умолчанию).
- Перенести тест поворота по умолчанию.

## Вне объёма
- Ввод постройки и ударов (только объявить действия не нужно).
- Привязка границ к сетке (TD-003).

## Файлы
- `Source/TDRuntime/{Public,Private}/Camera/*`, `Game/TD_PlayerController.*`, `Game/TD_GameMode.*`, тест камеры

## Действия владельца в Editor
1. Создать `DA_TD_CameraConfig` (значения как в GrimProtocol), `IMC_TD_Default` и `IA_TD_Camera*` по TDD/15.
2. На `L_TD_Empty` поставить плоскость 13×10 м и `ATD_CameraBoundsVolume` по её размеру.
3. Назначить `ATD_GameMode` в World Settings.

## Критерии приёмки
- [ ] Собирается TDEditor Win64 Development без новых предупреждений.
- [ ] Нет чисел баланса в C++.
- [ ] Нейминг по Coding_Rules.
- [ ] TDD обновлён: TDD/15 (что фактически перенесено).
- [ ] Камера двигается WASD, стрелками и у края экрана.
- [ ] Вращение средней кнопкой и Q/E (45°); зум колесом в пределах конфига.
- [ ] Камера не выходит за объём.
- [ ] В коде нет ссылок на Voxel, FoW, Minimap.
- [ ] Тест поворота проходит.

## Ручная проверка
PIE на `L_TD_Empty`: проверить все способы движения, вращения и зума, упор в границы.

## Риски
- Скрытые зависимости камеры GrimProtocol от других систем — сборка покажет.

## Отчёт
`Docs/Development/Reports/TD-002.md`: что сделано, файлы, как проверено, отклонения, вопросы.

## Стоп
После задачи — остановиться. Следующую не начинать без «готово».

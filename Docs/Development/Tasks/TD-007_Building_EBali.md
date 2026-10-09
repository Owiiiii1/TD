# TD-007 — Постройка, улучшение, продажа и е-бали

## Статус
Не начато

## Этап
M1 First Playable

## Зависит от
TD-006

## Цель
Игрок ставит, улучшает и продаёт юниты и ежи за е-бали; уничтожение врага даёт е-бали.

## Что прочитать перед началом
- `CLAUDE.md`, [Coding_Rules](../Coding_Rules.md), [Workflow](../Workflow.md)
- [GDD/12](../../GDD/12_Building.md), [GDD/30](../../GDD/30_Balance_Economy.md), [TDD/10](../../TDD/10_Grid_Routes_Building.md), [TDD/12](../../TDD/12_Level_Waves_Economy.md)

## Объём работ
- `ATD_GameState` (е-бали, делегаты), `UTD_BuildComponent` (режимы, призрак, причина запрета, Shift, ПКМ/Esc, улучшение, продажа 70%/100%).
- `ATD_RoadObstacle` + `UTD_RoadObstacleData` (ежи, замедление 40%).
- Награда за врага в е-бали.
- Действия ввода 1–9, Shift, ЛКМ, ПКМ/Esc, Alt (радиусы).
- Automation Test: списание, возврат, запреты.

## Вне объёма
- UI-панели (TD-009) — пока отладочный вывод.

## Файлы
- `Game/TD_GameState.*`, `Building/TD_BuildComponent.*`, `Units/TD_RoadObstacle.*`, `Data/TD_RoadObstacleData.*`, `Game/TD_PlayerController.*`, тест

## Действия владельца в Editor
Добавить IA и привязки постройки в `IMC_TD_Default`; создать `DA_TD_Obstacle_Hedgehogs`.

## Критерии приёмки
- [ ] Собирается TDEditor Win64 Development без новых предупреждений.
- [ ] Нет чисел баланса в C++.
- [ ] Нейминг по Coding_Rules.
- [ ] TDD обновлён: TDD/10, TDD/12.
- [ ] Все правила GDD/12 выполняются.
- [ ] Ежи только на дорогу, не на Spawn/Border.

## Ручная проверка
PIE: строить клавишами 1–5, продавать, улучшать; смотреть баланс в отладочном выводе.

## Риски
—

## Отчёт
`Docs/Development/Reports/TD-007.md`: что сделано, файлы, как проверено, отклонения, вопросы.

## Стоп
После задачи — остановиться. Следующую не начинать без «готово».

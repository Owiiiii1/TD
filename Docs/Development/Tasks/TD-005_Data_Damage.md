# TD-005 — Данные врагов и юнитов, таблица урона

## Статус
Не начато

## Этап
M1 First Playable

## Зависит от
TD-004

## Цель
Ввести DataAsset юнитов, врагов и таблицы урона и библиотеку урона по таблице.

## Что прочитать перед началом
- `CLAUDE.md`, [Coding_Rules](../Coding_Rules.md), [Workflow](../Workflow.md)
- [GDD/13](../../GDD/13_Defense_Units.md), [GDD/14](../../GDD/14_Targeting_Damage.md), [GDD/15](../../GDD/15_Enemies.md), [TDD/02](../../TDD/02_Data_Assets_And_Tags.md), [TDD/11](../../TDD/11_Combat_Units_Enemies.md)

## Объём работ
- `UTD_UnitData`, `UTD_EnemyData`, `UTD_DamageTableData` с полями из TDD/02 (поля особых свойств можно объявить, логика — M2), `IsDataValid`.
- `ATD_Enemy`: прочность из данных, теги, смерть, событие гибели с наградой.
- `UTD_DamageLibrary`: `ApplyDamage`, `ApplyAreaDamage`.
- `UTD_EnemyRegistry` внутри `ATD_GameMode`.
- Automation Test: урон по всем парам таблицы.

## Вне объёма
- Юниты и стрельба (TD-006).
- Е-бали (TD-007).

## Файлы
- `Data/TD_UnitData.*`, `Data/TD_EnemyData.*`, `Data/TD_DamageTableData.*`, `Combat/TD_DamageLibrary.*`, `Enemies/TD_Enemy.*`, `Game/TD_GameMode.*`, тест

## Действия владельца в Editor
Создать `DA_TD_DamageTable` по GDD/14 и данные врагов First_Playable: орк-мотострелок (+ вариант), БМП-2, Т-72Б3, Ка-52 — числа из GDD/15.

## Критерии приёмки
- [ ] Собирается TDEditor Win64 Development без новых предупреждений.
- [ ] Нет чисел баланса в C++.
- [ ] Нейминг по Coding_Rules.
- [ ] TDD обновлён: TDD/02, TDD/11.
- [ ] Урон по классам совпадает с таблицей.
- [ ] Враг с прочностью 0 погибает и сообщает о гибели.

## Ручная проверка
Консолью нанести урон врагу каждым типом, сравнить с таблицей.

## Риски
—

## Отчёт
`Docs/Development/Reports/TD-005.md`: что сделано, файлы, как проверено, отклонения, вопросы.

## Стоп
После задачи — остановиться. Следующую не начинать без «готово».

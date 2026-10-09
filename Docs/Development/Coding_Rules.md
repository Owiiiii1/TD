# Правила кода

## Именование

| Что | Шаблон | Пример |
| --- | --- | --- |
| Actor | `ATD_<Имя>` | `ATD_DefenseUnit` |
| UObject, компонент, DataAsset | `UTD_<Имя>` | `UTD_WeaponComponent`, `UTD_UnitData` |
| Struct | `FTD_<Имя>` | `FTD_WaveDef` |
| Enum | `ETD_<Имя>` | `ETD_CellType` |
| Interface | `ITD_<Имя>` | `ITD_Damageable` |
| Ассеты | `<Тип>_TD_<Имя>` | `DA_TD_Unit_Javelin`, `BP_TD_Enemy_T72`, `WBP_TD_HUD` |
| Теги | `TD.<Группа>.<Имя>` | `TD.Target.Armor` |
| Категории свойств | `TD|<Система>` | `TD|Combat` |

## Обязательно

- Стандарт кода Epic (отступы табами, `UPROPERTY` с категориями, `bIsSomething` для bool).
- Числа баланса — только в DataAsset (ADR-0004).
- Контент — мягкие ссылки.
- Теги — из `FTD_GameplayTags`, без `RequestGameplayTag("...")` в коде.
- Каждый DataAsset реализует `IsDataValid`.
- Логи — категория `LogTD` (+ подкатегории), без `LogTemp` в итоговом коде.
- Новая система — Automation Test на ключевые правила из GDD.

## Запрещено

- Сеть, репликация, GAS (ADR-0003).
- Subsystem без ADR (ADR-0005).
- Tick там, где хватает таймера или события.
- Файл больше ~1000 строк.
- Текст для игрока в C++ (только `FText` из String Tables).
- `TODO` без номера задачи.

## Сборка

Перед отчётом: `TDEditor Win64 Development` собирается без новых предупреждений; Automation Tests задачи проходят.

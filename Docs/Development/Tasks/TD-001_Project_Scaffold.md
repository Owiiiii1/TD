# TD-001 — Каркас проекта Unreal

## Статус
Не начато

## Этап
M0 Подготовка

## Зависит от
—

## Цель
Создать проект UE 5.8 с модулями TDRuntime и TDUIRuntime, плагинами и настройками репозитория, чтобы он собирался и открывался.

## Что прочитать перед началом
- `CLAUDE.md`, [Coding_Rules](../Coding_Rules.md), [Workflow](../Workflow.md)
- [TDD/00](../../TDD/00_Technical_Overview.md), [TDD/01](../../TDD/01_Module_Architecture.md), [TDD/16](../../TDD/16_Art_Pipeline.md) (раздел Git LFS)
- [ADR-0001](../../ADR/ADR_0001_Project_Prefix_TD.md), [ADR-0002](../../ADR/ADR_0002_Engine_UE58.md), [ADR-0003](../../ADR/ADR_0003_Singleplayer_No_GAS.md)

## Объём работ
- `TD.uproject` в корне репозитория, EngineAssociation 5.8.
- Модули `TD` (primary), `TDRuntime`, `TDUIRuntime` с Build.cs по TDD/01.
- Плагины: CommonUI, ModelViewViewModel.
- `FTD_GameplayTags` с тегами из TDD/02 (нативная регистрация).
- Лог-категория `LogTD`.
- `.gitignore` для Unreal и `.gitattributes` для Git LFS по TDD/16.
- Пустая карта `L_TD_Empty` как карта по умолчанию.
- В TDD/00 записать точную версию движка.

## Вне объёма
- Камера, геймплей, UI.
- Модуль `TDEditor`.

## Файлы
- `TD.uproject`, `Source/TD/*`, `Source/TDRuntime/*`, `Source/TDUIRuntime/*`, `Source/*.Target.cs`
- `Config/DefaultEngine.ini`, `Config/DefaultGame.ini`, `Config/DefaultGameplayTags.ini` (если нужно)
- `.gitignore`, `.gitattributes`

## Действия владельца в Editor
1. Установить UE 5.8 (последний патч), если не стоит.
2. Сгенерировать файлы проекта (ПКМ по `TD.uproject` → Generate Visual Studio project files).
3. Собрать TDEditor Win64 Development, открыть проект.
4. Убедиться, что Git LFS установлен (`git lfs install`).

## Критерии приёмки
- [ ] Собирается TDEditor Win64 Development без новых предупреждений.
- [ ] Нет чисел баланса в C++.
- [ ] Нейминг по Coding_Rules.
- [ ] TDD обновлён: TDD/00 (версия движка).
- [ ] Проект открывается в Editor без ошибок.
- [ ] Теги `TD.*` видны в Project Settings → Gameplay Tags.
- [ ] `git status` после сборки не показывает Binaries/Intermediate/Saved.

## Ручная проверка
Открыть проект, загрузить `L_TD_Empty`, запустить PIE — пустая сцена без ошибок в логе.

## Риски
- Версии плагинов MVVM/CommonUI в 5.8 — проверить, что включаются без конфликтов.

## Отчёт
`Docs/Development/Reports/TD-001.md`: что сделано, файлы, как проверено, отклонения, вопросы.

## Стоп
После задачи — остановиться. Следующую не начинать без «готово».

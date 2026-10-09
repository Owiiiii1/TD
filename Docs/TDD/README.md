# TDD — как это построено в Unreal

## Страницы

- [00_Technical_Overview](00_Technical_Overview.md) — стек, модули, карта классов, бюджет, конфигурация ПК.
- [01_Module_Architecture](01_Module_Architecture.md) — модули, папки, зависимости.
- [02_Data_Assets_And_Tags](02_Data_Assets_And_Tags.md) — все DataAsset и теги.
- [10_Grid_Routes_Building](10_Grid_Routes_Building.md) — сетка, маршруты, движение, постройка.
- [11_Combat_Units_Enemies](11_Combat_Units_Enemies.md) — юниты, враги, урон, особые угрозы, удары.
- [12_Level_Waves_Economy](12_Level_Waves_Economy.md) — уровень, волны, е-бали, рубеж, пауза.
- [13_Campaign_Save](13_Campaign_Save.md) — кампания, донаты, дерево, энциклопедия, сохранения.
- [14_UI](14_UI.md) — CommonUI, ViewModel, экраны, локализация.
- [15_Camera_Input](15_Camera_Input.md) — камера из GrimProtocol и Enhanced Input.
- [16_Art_Pipeline](16_Art_Pipeline.md) — структура контента, бюджеты, импорт, LFS.

## GDD ↔ TDD

| Система | GDD | TDD |
| --- | --- | --- |
| S01 Сетка, S02 Дороги, S03 Постройка | [10](../GDD/10_Map_Grid.md), [11](../GDD/11_Roads_Movement.md), [12](../GDD/12_Building.md) | [10_Grid_Routes_Building](10_Grid_Routes_Building.md) |
| S04 Юниты, S05 Урон, S06 Враги, S10–S17 | [13](../GDD/13_Defense_Units.md)–[15](../GDD/15_Enemies.md), [18](../GDD/18_Super_Strikes.md)–[27](../GDD/27_Tractor_Trophies.md) | [11_Combat_Units_Enemies](11_Combat_Units_Enemies.md) |
| S07 Волны, S08 Экономика, S09 Рубеж | [16](../GDD/16_Waves.md), [30](../GDD/30_Balance_Economy.md), [17](../GDD/17_Win_Lose.md) | [12_Level_Waves_Economy](12_Level_Waves_Economy.md) |
| S18 Поток, S19 Энциклопедия, S22 Дерево | [28](../GDD/28_Level_Campaign_Flow.md), [29](../GDD/29_Encyclopedia.md), [32](../GDD/32_Tech_Tree.md) | [13_Campaign_Save](13_Campaign_Save.md) |
| S20 UI, камера | [40](../GDD/40_UI_UX.md) | [14_UI](14_UI.md), [15_Camera_Input](15_Camera_Input.md) |
| S21 Арт | [50](../GDD/50_Art_Audio_Direction.md) | [16_Art_Pipeline](16_Art_Pipeline.md) |

## Правила

- TDD обновляется до или вместе с кодом, который меняет архитектуру.
- Нетривиальное решение — ADR, TDD на него ссылается.
- TDD не повторяет GDD: правила игры — там, реализация — здесь.

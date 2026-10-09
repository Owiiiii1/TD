# Технический обзор

**Статус:** черновик · **ADR:** [0001](../ADR/ADR_0001_Project_Prefix_TD.md)–[0010](../ADR/ADR_0010_AI_Assets_Registry.md)

Одиночная игра на Unreal Engine 5.8: геймплей на C++ в двух модулях, данные в DataAsset, UI на CommonUI + MVVM, без сети и без GAS.

## Стек

| Что | Решение |
| --- | --- |
| Движок | Unreal Engine 5.8.x (ADR-0002) |
| Язык | C++ (геймплей), Blueprint (визуал, вёрстка UI, наследники ассетов) |
| Сеть | Нет (ADR-0003) |
| Ввод | Enhanced Input |
| UI | CommonUI + ModelViewViewModel (ADR-0009) |
| Данные | DataAsset + Asset Manager + Gameplay Tags (ADR-0004) |
| Сохранение | `USaveGame`, один слот (ADR-0008) |
| Локализация | String Tables, uk + en |
| Платформа | Win64, Steam |
| Контроль версий | Git + Git LFS для бинарных ассетов |

## Модули

| Модуль | Тип | Владеет |
| --- | --- | --- |
| `TD` | Primary game module | Только загрузка проекта |
| `TDRuntime` | Runtime | Весь геймплей: сетка, маршруты, юниты, враги, урон, волны, экономика, кампания, камера |
| `TDUIRuntime` | Runtime | C++-база виджетов и ViewModel |
| `TDEditor` | Editor | Превью сетки и валидаторы — только когда понадобятся |

Подробно — [01_Module_Architecture](01_Module_Architecture.md).

## Карта классов (MVP)

| Слой | Классы |
| --- | --- |
| Игра | `ATD_GameMode` (уровень), `ATD_GameState` (е-бали, рубеж, волна), `ATD_PlayerController`, `UTD_CampaignSubsystem`, `UTD_SaveGame` |
| Карта | `ATD_LevelGrid`, `ATD_GridVolume`, `ATD_Route` |
| Оборона | `ATD_DefenseUnit`, `UTD_WeaponComponent`, `UTD_SuppressionComponent`, `UTD_TractorComponent`, `ATD_RoadObstacle` |
| Враги | `ATD_Enemy`, `UTD_RouteFollowerComponent`, `UTD_EnemyTraitComponent` (свойства), `ATD_Wreck` |
| Бой | `UTD_DamageLibrary` (функции урона), `ATD_ProjectileVisual` (пул), `UTD_SuperStrikeComponent` (на контроллере) |
| Поток | `UTD_WaveDirectorComponent` (на GameMode), `UTD_BuildComponent` (на контроллере) |
| Камера | `ATD_CameraPawn`, `UTD_CameraConfigDataAsset`, `ATD_CameraBoundsVolume` |
| Данные | см. [02_Data_Assets_And_Tags](02_Data_Assets_And_Tags.md) |
| UI | `UTD_*ViewModel`, `UTD_*Screen` — см. [14_UI](14_UI.md) |

## Поток кадра

1. Ввод (Enhanced Input) → камера, постройка, удары.
2. `UTD_WaveDirectorComponent` — таймеры и появление врагов.
3. Враги двигаются вдоль маршрутов (Tick врага).
4. Юниты выбирают цели и стреляют по таймеру перезарядки (без Tick).
5. Урон применяется в момент попадания (визуальный снаряд из пула летит заданное время).
6. Состояние (е-бали, рубеж) меняется в `ATD_GameState` → делегаты → ViewModel → виджеты.

## Бюджет производительности (MVP)

| Метрика | Цель |
| --- | --- |
| FPS | 60 на рекомендуемой конфигурации при пике |
| Враги одновременно | до 150 |
| Юниты одновременно | до 40 |
| Визуальные снаряды | до 300, пул |
| Tick | Только враги, камера, снаряды; юниты и ViewModel — по событиям и таймерам |
| Полигоны | Враг-пехота ≤ 3k, техника ≤ 10k, юнит ≤ 8k, босс ≤ 40k (ориентир) |

## Конфигурация ПК (предварительно)

| | Минимальная | Рекомендуемая |
| --- | --- | --- |
| ОС | Windows 10 64-bit | Windows 11 64-bit |
| CPU | 4 ядра, ~2018 г. | 6 ядер, ~2020 г. |
| GPU | GTX 1060 / RX 580 | RTX 2060 / RX 6600 |
| RAM | 8 ГБ | 16 ГБ |

Уточняется после замеров на первом играбельном билде.

## Ссылки

- [GDD/00_Overview](../GDD/00_Overview.md), [GDD/03_Systems_Index](../GDD/03_Systems_Index.md)

# Сетка, маршруты, постройка — архитектура

**Статус:** черновик · **GDD:** [10_Map_Grid](../GDD/10_Map_Grid.md), [11_Roads_Movement](../GDD/11_Roads_Movement.md), [12_Building](../GDD/12_Building.md) · **ADR:** [0007](../ADR/ADR_0007_Routes_And_Grid_Authoring.md)

## Обзор

`ATD_LevelGrid` знает типы и занятость клеток. `ATD_Route` — сплайн маршрута. `UTD_RouteFollowerComponent` двигает врага по сплайну. `UTD_BuildComponent` на контроллере управляет призраком, постройкой, улучшением и продажей.

## Классы

| Класс | Тип | Ответственность |
| --- | --- | --- |
| `ATD_LevelGrid` | Actor (1 на уровень) | Размер поля, клетки, типы, занятость; перевод мир ↔ клетка; отрисовка сетки в режиме постройки |
| `ATD_GridVolume` | Actor (Box) | Задаёт тип клеток под собой: Препятствие, Зона высадки (+ точка выхода и маршрут) |
| `ATD_Route` | Actor + `USplineComponent` | Тип (наземный/водный/воздушный), ширина в клетках, имя (для волн) |
| `UTD_RouteFollowerComponent` | ActorComponent на враге | Пройденная дистанция, скорость, замедления, разброс по ширине, направление (вперёд/назад) |
| `ATD_RoadObstacle` | Actor | Ежи и подобное; регистрирует зону замедления на клетке |
| `UTD_BuildComponent` | ActorComponent на `ATD_PlayerController` | Режимы, призрак, проверка, постройка, улучшение, продажа |

## Данные

- `ATD_LevelGrid`: `FIntPoint GridSize`, `float CellSizeCm` (из `DA_TD_GameSettings`, 400), `TArray<ETD_CellType> Cells`, `TArray<TWeakObjectPtr<AActor>> Occupants`.
- `ETD_CellType`: Ground, Road, Water, Blocked, LandingZone, Spawn, Border.
- Растеризация при `BeginPlay` (и по кнопке в Editor): маршруты → Road/Water по ширине; объёмы → свои типы; начало маршрута → Spawn, конец → Border; остальное → Ground.

## Поток

**Постройка:**
1. UI или клавиша → `UTD_BuildComponent::BeginPlacement(UnitData)`.
2. Каждый кадр в режиме: курсор → клетка → `CanPlace(UnitData, Cell)` → цвет призрака и причина.
3. ЛКМ → `TryPlace` → списать е-бали в `ATD_GameState` → заспавнить `ATD_DefenseUnit` → `ATD_LevelGrid::Occupy`.
4. Продажа → `ATD_GameState::AddEBali(возврат)` → `Release` → уничтожить актор. Флаг «построен в текущей паузе между волнами» хранится на юните для 100% возврата.

**Движение врага:**
`Distance += Speed × (1 − MaxActiveSlow) × DeltaTime × Direction`; позиция = `Spline.GetLocationAtDistance(Distance) + Right × LaneOffset`. По достижении конца → событие «дошёл» в GameMode. Прогресс = Distance / Length.

**Замедление:** `ATD_RoadObstacle` и удар «ведьмы» добавляют врагу запись замедления (источник, сила, срок); компонент берёт максимум.

## Производительность

- Поиск клетки — O(1) по координатам.
- Нет навмеша и поиска пути.
- Враги не проверяют столкновения друг с другом.

## Анти-паттерны

- Не держать список врагов в сетке.
- Не использовать навмеш для врагов.
- Не хранить е-бали в контроллере — только в `ATD_GameState`.

## Тесты и проверка

- Automation: растеризация тестового уровня даёт ожидаемые типы клеток; `CanPlace` для всех случаев из GDD; возврат 70% / 100%.
- Ручная: тестовая карта, сетка видна в постройке, враги едут по двум маршрутам.

## Риски

| Риск | Митигация |
| --- | --- |
| Широкий сплайн неточно растеризуется на поворотах | Шаг выборки ¼ клетки, превью в Editor |
| Клик по клетке через 3D-рельеф | Трассировка в плоскость сетки, а не по ландшафту |

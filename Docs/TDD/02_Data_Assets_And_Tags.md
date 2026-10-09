# DataAsset и Gameplay Tags

**Статус:** черновик · **ADR:** [0004](../ADR/ADR_0004_Data_Driven.md)

Все числа баланса и контент — в DataAsset. Классы, типы и свойства — в Gameplay Tags. Каждый DataAsset проверяет себя в `IsDataValid`.

## DataAsset

| Класс | Ассет | Поля (основные) | GDD |
| --- | --- | --- | --- |
| `UTD_DamageTableData` | `DA_TD_DamageTable` | Множитель для каждой пары (тип урона, класс цели) | S05 |
| `UTD_UnitData` | `DA_TD_Unit_<Имя>` | Имя (Text), роль (Text), размер (1×1/2×2), тег типа урона, урон, перезарядка, радиус, мин. дальность, радиус взрыва, цена, цены улучшений, бьёт дроны/вертолёты, наблюдатель, свойство уровня III, класс актора (soft), иконка (soft), запись энциклопедии | S04 |
| `UTD_RoadObstacleData` | `DA_TD_Obstacle_<Имя>` | Цена, замедление, класс актора | S02, S04 |
| `UTD_EnemyData` | `DA_TD_Enemy_<Имя>` | Имя, тег класса цели, теги свойств, прочность, скорость, награда, урон рубежу, может отступить и шанс, варианты (данные + шанс), обломки (да/нет), шанс трофея и юнит-трофей, параметры свойства (подавление, высадка), класс актора (soft) | S06, S11–S17 |
| `UTD_BossData` | наследник `UTD_EnemyData` | Свойство босса, интро-текст | S15 |
| `UTD_SuperStrikeData` | `DA_TD_Strike_<Имя>` | Тип эффекта, урон и тег урона, радиус, задержка, длительность, перезарядка, лимит за уровень, горячая клавиша по умолчанию | S10 |
| `UTD_LevelData` | `DA_TD_Level_<NN>` | Карта (soft world), номер, этап, название, место, дата, тексты брифинга и справки (uk/en), что нового, стартовые е-бали, рубеж, таймер между волнами, волны, ключевые выдачи, точка на карте кампании | S07, S18 |
| `FTD_WaveDef` / `FTD_WaveGroupDef` | внутри уровня | Группы: враг, количество, имя маршрута, интервал, задержка | S07 |
| `UTD_TechTreeData` | `DA_TD_TechTree` | Узлы: id, ветка, тип (юнит/удар/ветеран), ссылка, цена, доступен с уровня | S22 |
| `UTD_EncyclopediaEntryData` | `DA_TD_Entry_<Имя>` | Раздел, тексты uk/en, факты, мем-подпись, источник фактов, модель для превью | S19 |
| `UTD_GameSettingsData` | `DA_TD_GameSettings` | Экономика (возврат, бонусы), мораль, трофеи, звёзды, донаты, макс. замедление, длительности обнаружения | S08, S09, S16, S17, S22 |
| `UTD_CameraConfigDataAsset` | `DA_TD_CameraConfig` | Из GrimProtocol | S20 |

## Gameplay Tags

```
TD.Target.Infantry / LightVehicle / Armor / Air / Naval
TD.Target.Drone                         ← дополнительная пометка для Air
TD.Damage.SmallArms / HighExplosive / AntiTank / AntiAir / AntiShip
TD.Trait.Stealth / Paratrooper / Suppressor / DismountOnDeath / LootDrop / Retreats / FlyToBorder / StrikeRun / Transport
TD.Unit.Observer / HitsDrones / HitsHelicopters
TD.Strike.Damage / Slow / Reveal / AirWipe
TD.Status.Suppressed / Slowed / Revealed / Retreating
```

Теги объявляются нативно в `FTD_GameplayTags` (C++), без строк в коде.

## Asset Manager

- Primary Asset Types: `TDLevel`, `TDUnit`, `TDEnemy`, `TDStrike`, `TDEntry`.
- При старте уровня загружаются данные уровня и всё, на что ссылаются его волны и открытые юниты.

## Валидация

Каждый DataAsset в `IsDataValid` проверяет обязательные поля, диапазоны (например, множители 0–3) и мягкие ссылки. Уровень проверяет, что все маршруты из волн существуют на карте (при открытии карты в Editor).

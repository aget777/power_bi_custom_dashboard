# Plan for Рекламная активность - общие настройки страницы

Spec: `specs/ad-activity-page-general-settings.md`

## Goal
Реализовать утвержденный базовый слой страницы Power BI `Рекламная активность`: фон и цвета по референсу `screenshots/00_40_screen.jpg`, заголовок, фильтр по датам с дефолтным периодом `27.04.2026 - 03.05.2026` и правый верхний индикатор выбранного диапазона дат.

## Scope
- Используем существующую страницу `test.Report/definition/pages/5752fcd0093969ea3e03`.
- Переименовываем страницу с `test` на `Рекламная активность`.
- Сохраняем фактический размер страницы после правок в Power BI: `1500 x 1100` и `FitToPage`.
- Сохраняем page-level фильтры, добавленные в Power BI:
  - `переключатель_слева[Показатель] = TVR`;
  - `nat_tv_prj_union_dict[prj_name_rus] = ALL_18_60_BC_INC_GR`.
- Не удаляем новую страницу `test.Report/definition/pages/cca46bf570d5dd618f6d`, добавленную Power BI, пока нет отдельного решения по структуре страниц.
- Не применяем ограничение календаря по Adex.
- Не добавляем отдельную брендовую палитру или брендовый фон.
- В `_Меры_30_Presentation` появились презентационные меры для верхней шапки:
  - `3301_01_Выбранная_аудитория_презентация`;
  - `3302_01_Номер_выбранной_недели_пр`;
  - `3303_01_Начало_выбранной_недели`;
  - `3303_02_Конец_выбранной_недели`;
  - `3303_03_Начало_конец_периода`.

## Implementation Steps

### 1. Подготовить страницу
- Обновить `test.Report/definition/pages/5752fcd0093969ea3e03/page.json`:
  - `displayName`: `Рекламная активность`;
  - `displayOption`: оставить `FitToPage`;
  - `height`: оставить `1100`;
  - `width`: оставить `1500`;
  - добавить/проверить page background `#0A0E19`, если это поддерживается в JSON страницы.
- Удалить или заменить текущие тестовые визуалы на странице, если они не относятся к утвержденному базовому слою.

### 2. Добавить меру выбранного диапазона
- Обновить `test.SemanticModel/definition/tables/_Меры_29_Текст_и_Даты.tmdl`.
- Добавить меру `2901_01_06_текст_выбранный_диапазон` в папку `29_01_01_Даты`.
- В DAX переиспользовать существующие меры:
  - `2901_01_01_Мин_текущая_дата`;
  - `2901_01_02_Макс_дата_календарь`.
- Формат результата:
  - нет даты: `Период не выбран`;
  - один день: `dd.MM.yyyy`;
  - диапазон: `dd.MM.yyyy - dd.MM.yyyy`.

### 3. Добавить заголовок страницы
- Создать новый `textbox` visual на странице `5752fcd0093969ea3e03`.
- Текст: `Рекламная активность`.
- Координаты: `x=32`, `y=24`, `w=560`, `h=36`.
- Стиль:
  - `Segoe UI`;
  - размер `24`;
  - semibold/bold;
  - цвет `#E2C16F`;
  - фон, рамка и visual header выключены.

### 4. Добавить фильтр по датам
- Создать `slicer` visual по полю `'Календарь'[Дата]`.
- Режим: date range / between.
- Заголовок: `Период`.
- Фактические координаты: `x=31.94`, `y=148.10`, `w=341.21`, `h=56.63`.
- Настроить дефолтный выбранный период `27.04.2026 - 03.05.2026` через состояние slicer/filter в JSON.
- Не использовать меру `2901_01_05_adex_calendar_filter`.

### 5. Добавить правый верхний индикатор периода
- Создать `cardVisual` или другой текстовый визуал, привязанный к мере `2901_01_06_текст_выбранный_диапазон`.
- Координаты: `x=920`, `y=24`, `w=328`, `h=32`.
- Стиль:
  - выравнивание справа;
  - `Segoe UI`;
  - размер `13`;
  - цвет `#9AA3B2`;
  - category label off;
  - фон, рамка и visual header выключены.

### 6. Проверить page metadata
- Проверить `test.Report/definition/pages/pages.json`:
  - страница `5752fcd0093969ea3e03` остается в `pageOrder`;
  - `activePageName` можно оставить `5752fcd0093969ea3e03`.

### 7. Валидация
- Проверить JSON на синтаксическую корректность.
- Проверить TMDL на сохранение структуры и отступов существующего файла.
- Открыть PBIP в Power BI Desktop и проверить:
  - имя страницы `Рекламная активность`;
  - фон `#0A0E19`;
  - заголовок в левом верхнем углу;
  - date slicer показывает период `27.04.2026 - 03.05.2026`;
  - индикатор справа показывает `27.04.2026 - 03.05.2026`;
  - при изменении slicer индикатор обновляется;
  - элементы верхней панели не перекрываются.

## Files To Change
- `test.Report/definition/pages/5752fcd0093969ea3e03/page.json`
- `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/*/visual.json`
- `test.SemanticModel/definition/tables/_Меры_29_Текст_и_Даты.tmdl`
- possibly `test.Report/definition/pages/pages.json`, only if active page metadata needs adjustment.

## Reference Existing Patterns
- Existing test page slicers:
  - `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/e09e095fb8fc2aad545d/visual.json`
  - `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/016321ba7980b21ba149/visual.json`
  - `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/1c787557cb67185f25c4/visual.json`
- Existing date slicer examples:
  - `test.Report/definition/pages/bc52d52ead1b06e609ae/visuals/7fd08d8aa19a8e5df868/visual.json`
- Existing textbox examples:
  - `test.Report/definition/pages/e60b44142148dfd5c8d0/visuals/5cc4052738161890ddd0/visual.json`
  - `test.Report/definition/pages/bc52d52ead1b06e609ae/visuals/9f5304172acd04ca6851/visual.json`
- Existing card examples:
  - `test.Report/definition/pages/e60b44142148dfd5c8d0/visuals/a919c838b604097adc41/visual.json`
  - `test.Report/definition/pages/bc52d52ead1b06e609ae/visuals/dd3a57a920e405fbcb07/visual.json`

## Risks / Notes
- PBIP visual JSON for date range slicer can be sensitive to exact object structure. Prefer copying an existing date slicer pattern and changing field, position, header and filter values.
- Static default period is acceptable only as initial slicer/filter state. The visible right-side text must always be calculated by measure.
- If page-level background is not represented in `page.json`, use a full-page rectangle/shape only if Power BI Desktop confirms it renders behind all visuals.

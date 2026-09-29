# Plan for Рекламная активность - Топ 5 Розница

Spec: `specs/ad-activity-top5-retail.md`

## Goal
Добавить правый верхний HTML-блок `ТОП-5 • РОЗНИЦА` по аналогии с `ТОП-5 • ОБЩАЯ РЕКЛАМА`, включая строковые SOV и нижний блок продуктовых сегментов.

## Implementation Steps

### 1. Добавить HTML-меру
- Обновить `test.SemanticModel/definition/tables/_Меры_30_Presentation.tmdl`.
- Добавить меру `3202_02_html_top5_retail`.
- Использовать:
  - `3122_01_TPR_Розница` для рейтинга, значения и бара;
  - `3121_02_SOV_%_category_5_n_6` для SOV справа от бара;
  - `3122_02_SOV_%_Розница` для процента в заголовке;
  - `logos_dict[logo_link]` для логотипов;
  - `brand_main_Top[brand_main_top]` для названий брендов.

### 2. Добавить продуктовые сегменты
- В той же HTML-мере вывести блок `ПРОДУКТОВЫЕ СЕГМЕНТЫ`.
- Значения:
  - `3123_02_SOV_%_Розница_Дебетовые_карты`;
  - `3124_02_SOV_%_Розница_Сбер_продукты`;
  - `3125_02_SOV_%_Розница_Реф_программа`.
- Формат значений: `0%`, blank показывать как `-`.

### 3. Добавить visual на страницу
- Создать `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/aa100000000000000008/visual.json`.
- Тип visual: `htmlContent443BE3AD55E043BF878BED274D3A6855`.
- Query projection: `_Меры_30_Presentation[3202_02_html_top5_retail]`.
- Позиция: правая колонка напротив общей рекламы, высота увеличена под сегменты.

### 4. Проверка
- Проверить JSON через `ConvertFrom-Json`.
- Проверить, что мера и visual доступны по поиску в проекте.
- Открыть PBIP в Power BI Desktop для визуальной проверки HTML, загрузки логотипов и расчетов DAX.

## Files To Change
- `test.SemanticModel/definition/tables/_Меры_30_Presentation.tmdl`
- `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/aa100000000000000008/visual.json`
- `specs/ad-activity-top5-retail.md`
- `plans/ad-activity-top5-retail-plan.md`

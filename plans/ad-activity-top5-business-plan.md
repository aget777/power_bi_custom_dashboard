# Plan for Рекламная активность - Топ 5 Бизнес

Spec: `specs/ad-activity-top5-business.md`

## Goal
Добавить правый HTML-блок `ТОП • БИЗНЕС` по аналогии с `ТОП-5 • РОЗНИЦА`, включая строковые SOV и нижний блок бизнес-сегментов.

## Implementation Steps

### 1. Добавить HTML-меру
- Обновить `test.SemanticModel/definition/tables/_Меры_30_Presentation.tmdl`.
- Добавить меру `3202_03_html_top5_business`.
- Использовать:
  - `3132_01_TPR_Бизнес` для рейтинга, значения и бара;
  - `3121_02_SOV_%_category_5_n_6` для SOV справа от бара;
  - `3132_02_SOV_%_Бизнес` для процента в заголовке;
  - `logos_dict[logo_link]` для логотипов;
  - `brand_main_Top[brand_main_top]` для названий брендов.
- Фильтровать строки: `3132_01_TPR_Бизнес` не blank и `>= 1`.

### 2. Добавить бизнес-сегменты
- В той же HTML-мере вывести блок `БИЗНЕС-СЕГМЕНТЫ`.
- Значения:
  - `3133_02_SOV_%_Бизнес_Бухгалтерия`;
  - `3134_02_SOV_%_Бизнес_Платежи_и_переводы`;
  - `3135_02_SOV_%_Бизнес_Кредиты`.
- Формат значений: `0%`, blank показывать как `-`.

### 3. Добавить visual на страницу
- Создать `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/aa100000000000000009/visual.json`.
- Тип visual: `htmlContent443BE3AD55E043BF878BED274D3A6855`.
- Query projection: `_Меры_30_Presentation[3202_03_html_top5_business]`.
- Позиция: правая колонка под блоком `ТОП-5 • РОЗНИЦА`.

### 4. Проверка
- Проверить JSON через `ConvertFrom-Json`.
- Проверить, что мера и visual доступны по поиску в проекте.
- Открыть PBIP в Power BI Desktop для визуальной проверки HTML, логотипов, фильтров и расчетов DAX.

## Files To Change
- `test.SemanticModel/definition/tables/_Меры_30_Presentation.tmdl`
- `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/aa100000000000000009/visual.json`
- `specs/ad-activity-top5-business.md`
- `plans/ad-activity-top5-business-plan.md`

# Plan for Рекламная активность - Новые кампании

Spec: `specs/ad-activity-new-campaigns.md`

## Goal
Добавить на страницу `Рекламная активность` HTML-блок `НОВЫЕ КАМПАНИИ` со строками вида `NEW` + название кампании.

## Scope
- Используем страницу `test.Report/definition/pages/5752fcd0093969ea3e03`.
- Используем visual type `htmlContent443BE3AD55E043BF878BED274D3A6855`.
- Статус берем из `_Меры_30_Presentation[3110_01_New_campaigns]`.
- Название кампании берем из `campaigns_dict[campaign_name]`.
- Категорию кампании берем из `campaigns_dict[campaign_category]` для сортировки.

## Implementation
- Добавить меру `_Меры_30_Presentation[3210_01_html_new_campaigns]`.
- Мера строит список кампаний из `ALLSELECTED('campaigns_dict'[campaign_name])`.
- Для каждой кампании рассчитывает `[3110_01_New_campaigns]`.
- Для каждой кампании подтягивает `campaigns_dict[campaign_category]`.
- Исключает строки с blank/пустым статусом.
- Сортирует строки по категории, затем по названию кампании.
- Возвращает HTML карточки с заголовком `НОВЫЕ КАМПАНИИ`, зеленым акцентом и строками кампаний.
- Добавить visual container:
  - `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/aa100000000000000006/visual.json`;
  - query projection: `_Меры_30_Presentation[3210_01_html_new_campaigns]`;
  - позиция: `x=31.35`, `y=489.84`, `w=715.16`, `h=250.80`.

## Validation
- Проверить JSON нового visual container.
- Открыть PBIP в Power BI Desktop и проверить:
  - блок отображается под Top-5;
  - строки показывают `NEW` и название кампании;
  - блок реагирует на фильтры страницы;
  - при отсутствии новых кампаний отображается `Нет новых кампаний`.

## Files Changed
- `test.SemanticModel/definition/tables/_Меры_30_Presentation.tmdl`
- `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/aa100000000000000006/visual.json`

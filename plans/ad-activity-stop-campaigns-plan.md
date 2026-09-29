# Plan for Рекламная активность - Завершились кампании

Spec: `specs/ad-activity-stop-campaigns.md`

## Goal
Добавить на страницу `Рекламная активность` HTML-блок `ЗАВЕРШИЛИСЬ КАМПАНИИ` со строками вида `STOP` + название кампании, сгруппированными в колонки `Розница` и `Бизнес`.

## Scope
- Используем страницу `test.Report/definition/pages/5752fcd0093969ea3e03`.
- Используем visual type `htmlContent443BE3AD55E043BF878BED274D3A6855`.
- Статус берем из `_Меры_30_Presentation[3110_02_Stop_campaigns]`.
- Название кампании берем из `campaigns_dict[campaign_name]`.
- Категорию кампании берем из `campaigns_dict[campaign_category]`.

## Implementation
- Добавить меру `_Меры_30_Presentation[3210_02_html_stop_campaigns]`.
- Мера строит список кампаний из `campaigns_dict[campaign_name]` и `campaigns_dict[campaign_category]`.
- Для каждой кампании рассчитывает `[3110_02_Stop_campaigns]`.
- Исключает строки с blank/пустым статусом.
- Разделяет строки на `РОЗНИЦА` и `БИЗНЕС`.
- Возвращает HTML карточки с заголовком `ЗАВЕРШИЛИСЬ КАМПАНИИ`, красным акцентом и двумя колонками кампаний.
- Добавить visual container:
  - `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/aa100000000000000007/visual.json`;
  - query projection: `_Меры_30_Presentation[3210_02_html_stop_campaigns]`;
  - позиция: `x=31.35`, `y=765`, `w=715.16`, `h=300`.

## Validation
- Проверить JSON нового visual container.
- Открыть PBIP в Power BI Desktop и проверить:
  - блок отображается под `НОВЫЕ КАМПАНИИ`;
  - строки показывают `STOP` и название кампании;
  - строки распределены по колонкам `Розница` и `Бизнес`;
  - блок реагирует на фильтры страницы.

## Files Changed
- `test.SemanticModel/definition/tables/_Меры_30_Presentation.tmdl`
- `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/aa100000000000000007/visual.json`

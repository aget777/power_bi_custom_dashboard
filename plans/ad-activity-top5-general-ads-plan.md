# Plan for Рекламная активность - Топ 5 Общая реклама

Spec: `specs/ad-activity-top5-general-ads.md`

## Goal
Добавить на страницу `Рекламная активность` первый аналитический блок `ТОП-5 • ОБЩАЯ РЕКЛАМА`: темную карточку с пятью брендами, логотипами, значениями текущего выбранного показателя и горизонтальными барами по мере `400_00_12_Nat_Big_switch_left`.

## Scope
- Используем страницу `test.Report/definition/pages/5752fcd0093969ea3e03`.
- Используем существующий кастомный visual type `htmlContent443BE3AD55E043BF878BED274D3A6855`.
- Бренды берем из `brand_main_Top[brand_main_top]`.
- Логотипы берем из `logos_dict[logo_link]`.
- Значения берем из меры `[400_00_12_Nat_Big_switch_left]`.
- SOV показываем справа от бара по мере `3102_01_SOV_%`.

## Implementation Steps

### 1. Добавить HTML-меру
- Обновить `test.SemanticModel/definition/tables/_Меры_30_Presentation.tmdl`.
- Использовать меру `3202_01_html_top5_general_ads`.
- Мера должна:
  - построить таблицу брендов из `ALLSELECTED('brand_main_Top'[brand_main_top])`;
  - рассчитать значение `[400_00_12_Nat_Big_switch_left]`;
  - рассчитать SOV `[3102_01_SOV_%]`;
  - подтянуть логотип через связанный `logos_dict[logo_link]`;
  - исключить `ОСТАЛЬНОЕ`;
  - исключить blank и 0;
  - выбрать `TOPN(5, ..., [value], DESC, brand, ASC)`;
  - рассчитать максимум среди Top 5 для ширины бара;
  - вернуть одну HTML-строку.
- Формат значения:
  - подпись брать из `переключатель_слева[Показатель]`;
  - использовать `#,0.00` для показателей из `списокДесятичных`;
  - иначе использовать `#,0`.

### 2. Сверить DAX с моделью
- Проверить, что мера может ссылаться на:
  - `'brand_main_Top'[brand_main_top]`;
  - `'logos_dict'[logo_link]`;
  - `[400_00_12_Nat_Big_switch_left]`.
- Для логотипа использовать `CALCULATE(SELECTEDVALUE('logos_dict'[logo_link]))` в контексте текущего бренда.
- Если логотип пустой, формировать fallback-блок с первой буквой бренда.

### 3. Добавить HTML visual на страницу
- Создать новый visual container:
  - папка: `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/<new-id>/visual.json`;
  - visual type: `htmlContent443BE3AD55E043BF878BED274D3A6855`;
  - query projection: `_Меры_30_Presentation[3202_01_html_top5_general_ads]`.
- Рекомендуемый ID: `aa100000000000000005`.
- Позиция:
  - `x=31.94`;
  - `y=262.81`;
  - `w=714.37`;
  - `h=230.86`;
  - `z=4`;
  - `tabOrder=4`.
- Выключить visual header, background и border контейнера, потому что фон и рамка будут внутри HTML.

### 4. Стили HTML
- В HTML задать контейнер карточки:
  - background: `#0F1722`;
  - border: `1px solid #1C2739`;
  - border-radius: `8px`;
  - width/height: 100%;
  - box-sizing: border-box;
  - padding: 16-20px.
- Заголовок:
  - текст: `ТОП-5 • ОБЩАЯ РЕКЛАМА`;
  - цвет: `#E3EAF2`;
  - иконка/символ слева золотым `#E2C16F`;
  - letter spacing допускается только в HTML, если визуально соответствует референсу.
- Строки:
  - 5 строк фиксированной высоты;
  - номер: `#9AA3B2`;
  - логотип: `32 x 32`;
  - бренд: `#E3EAF2`;
  - значение: `#9AA3B2`;
  - bar track: `#1C2739`;
  - bar fill: `#E2C16F`.
  - SOV: `#9AA3B2`, справа от бара, формат `0% SOV`, обычный вес.
- Для длинных брендов использовать CSS `white-space: nowrap; overflow: hidden; text-overflow: ellipsis;`.

### 5. Проверить filter context
- Убедиться, что блок реагирует на date slicer `Календарь[Дата]`.
- Убедиться, что блок реагирует на brand slicer `brand_main_Top[brand_main_top]`.
- Не использовать `ALL('brand_main_Top')`, чтобы не сбрасывать выбранные бренды.
- Использовать `ALLSELECTED('brand_main_Top')` только для ранжирования в текущем пользовательском выборе.

### 6. Валидация
- Проверить JSON на синтаксическую корректность.
- Проверить visual JSON по schema.
- Проверить, что новая мера есть в `_Меры_30_Presentation.tmdl`.
- Открыть PBIP в Power BI Desktop и проверить:
  - блок отображается под фильтрами;
  - заголовок блока виден;
  - показывается максимум 5 брендов;
  - логотипы загружаются из URL;
  - значения отображаются с подписью текущего выбранного показателя;
  - SOV отображается справа от бара;
  - сортировка по значению убывающая;
  - фильтры дат и брендов меняют состав Top 5;
  - при отсутствии данных отображается `Нет данных`.

## Files To Change
- `test.SemanticModel/definition/tables/_Меры_30_Presentation.tmdl`
- `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/aa100000000000000005/visual.json`

## Reference Existing Patterns
- Existing HTML visual:
  - `test.Report/definition/pages/e60b44142148dfd5c8d0/visuals/308ec6662de20bc4ce6f/visual.json`
- Existing HTML measures:
  - `_Меры_30_Presentation.tmdl`, folder `29_02_HTML\03_Ad_activity`
- Existing page visual style:
  - `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/aa100000000000000001/visual.json`
  - `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/aa100000000000000002/visual.json`
  - `test.Report/definition/pages/5752fcd0093969ea3e03/visuals/aa100000000000000004/visual.json`

## Risks / Notes
- DAX string generation can become hard to read; keep the HTML compact but structured with variables.
- HTML visual may sanitize some CSS. Prefer simple inline CSS and basic tags.
- `logos_dict[logo_link]` depends on the relationship to `brand_main_Top`; if a logo does not appear, validate `logos_dict[brand_name]` text matches `brand_main_Top[brand_main_top]`.
- Keep the visual projection and filter field in sync with `_Меры_30_Presentation[3202_01_html_top5_general_ads]`; Power BI can keep stale `queryRef` values after manual edits.

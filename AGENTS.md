# Repository Guidelines

## Project Structure & Module Organization

This repository contains a Power BI Project (`test.pbip`).

- `test.Report/` contains report layout metadata, pages, bookmarks, visuals, and static resources.
- `test.SemanticModel/` contains the semantic model definition, TMDL tables, measures, relationships, cultures, and local model settings.
- `specs/` stores feature specifications, for example `specs/ad-activity-top5-business.md`.
- `plans/` stores implementation plans paired with specs.
- `screenshots/` contains visual references used for layout and styling work.

Most report behavior changes are made in TMDL measure files under `test.SemanticModel/definition/tables/` and visual bindings under `test.Report/definition/pages/.../visuals/`.

## Build, Test, and Development Commands

There is no package manager, build script, or automated test runner configured in this folder.

Useful local checks:

```powershell
rg -n "measure 3202" test.SemanticModel/definition/tables
rg -n "nativeQueryRef" test.Report/definition/pages
Get-ChildItem -Recurse test.Report, test.SemanticModel
```

Open `test.pbip` in Power BI Desktop to validate model loading, visual rendering, filters, and formatting.

## Coding Style & Naming Conventions

Preserve existing PBIP and TMDL formatting. Use tabs and indentation consistent with nearby measures. Keep DAX measure names aligned with the existing numeric prefixes, for example `3202_03_html_top5_business`.

For feature documentation, use kebab-case file names under `specs/` and `plans/`, matching the feature area:

```text
specs/ad-activity-top5-retail.md
plans/ad-activity-top5-retail-plan.md
```

Keep HTML-in-DAX measures self-contained and avoid changing shared calculation measures when only display behavior is required.

## Testing Guidelines

Validate changes in Power BI Desktop. At minimum, check that:

- `test.pbip` opens without model or report definition errors.
- Modified visuals render on the `Рекламная активность` page.
- Filters and slicers still affect the visual as expected.
- Display-only exclusions do not change denominator or aggregate calculations.

When editing JSON or TMDL manually, run targeted `rg` checks to confirm visual bindings and measure names.

## Commit & Pull Request Guidelines

This folder currently has no Git history available, so no existing commit convention can be inferred. Use concise imperative commits if this project is placed under Git, for example:

```text
Exclude Ozon bank brand from top-5 visuals
```

Pull requests should include a short summary, affected PBIP files, validation steps performed in Power BI Desktop, and screenshots for visual layout changes.

## Agent-Specific Instructions

Do not revert unrelated report or model changes. Keep edits narrow, especially in generated PBIP JSON. Prefer updating existing measures and visuals over introducing parallel copies unless the feature requires a new visual.

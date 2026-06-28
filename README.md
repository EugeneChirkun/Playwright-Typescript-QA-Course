# Playwright TypeScript QA Course

## Repository purpose

This repository contains the documentation source for a Russian-language course that helps experienced Manual QA engineers move to Automation QA with TypeScript and Playwright.

## GitHub Pages site

TODO: add GitHub Pages URL after the first deployment.

## Target audience

Опытные Manual QA engineers, которые понимают QA-процессы и хотят изучить automation с TypeScript, Playwright, Git, CI и практической структурой тестового проекта.

## Expected result after 3 months

Через 3 месяца студенты смогут создать базовый Playwright project, писать UI и API-тесты, использовать Page Object Model, fixtures, test data, Git branches, commits и pull requests.

## Repository structure

```text
README.md                  # Maintainer/contributor overview
AGENTS.md                  # Instructions for Codex and other coding agents
course.yml                 # Source of truth for course metadata
mkdocs.yml                 # MkDocs navigation and site configuration
requirements.txt           # Python dependencies for local site and CI
docs/                      # Student-facing documentation content
  index.md                 # Student-facing homepage
  learning-format.md       # Learning process description
  course-structure.md      # Таблица курса из 12 модулей
  modules/                 # Страницы модулей для MkDocs
templates/module/          # Шаблоны для новых модулей
.github/workflows/         # GitHub Pages deployment workflow
```

## Run documentation site locally

```bash
pip install -r requirements.txt
mkdocs serve
```

To verify the production build locally:

```bash
mkdocs build --strict
```


## CI and deployment

- CI runs on pull requests and pushes to `master`.
- Documentation deploy runs after push to `master`.
- GitHub Pages source should be set to GitHub Actions.
- Local build command: `mkdocs build --strict`.
- Local preview command: `mkdocs serve`.

## Добавить или обновить модуль

1. Обновите метаданные модуля в `course.yml`.
2. Добавьте или отредактируйте материалы для студентов в `docs/modules/<module-folder>/`.
3. Убедитесь, что у модуля есть `index.md`, `homework.md`, `checklist.md` и `resources.md`.
4. Обновите навигацию в `mkdocs.yml`, если страницы добавлены, удалены или переименованы.
5. Добавляйте внешние ссылки только в соответствующий `resources.md` модуля.

## How Codex should be used

Используйте Codex для небольших изменений документации, обновления структуры, навигации и проверки консистентности. Codex должен сохранять русские объяснения, оставлять общепринятые English technical terms там, где это естественно, и не делать большие переписывания без явного запроса.

## Student-facing content

Student-facing course content is located in `docs/`. The old top-level `modules/` directory is no longer used because MkDocs reads pages from `docs/`.

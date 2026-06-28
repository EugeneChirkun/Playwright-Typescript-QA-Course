# Playwright TypeScript QA Course

## Назначение repository

Этот repository содержит исходники документации для русскоязычного курса, который помогает опытным Manual QA engineers перейти в Automation QA с TypeScript и Playwright.

## GitHub Pages site

TODO: добавить URL GitHub Pages после первого deployment.

## Целевая аудитория

Курс рассчитан на опытных Manual QA engineers, которые понимают QA-процессы и хотят изучить автоматизацию с TypeScript, Playwright, Git, CI и практической структурой тестового project.

## Ожидаемый результат через 3 месяца

Через 3 месяца студенты смогут создавать базовый Playwright project, писать UI и API тесты, использовать Page Object Model, fixtures, test data, Git branches, commits и pull requests.

## Структура repository

```text
README.md                  # Обзор для maintainers и contributors
AGENTS.md                  # Инструкции для Codex и других coding agents
course.yml                 # Source of truth для metadata курса
mkdocs.yml                 # Навигация MkDocs и конфигурация сайта
requirements.txt           # Python dependencies для локального сайта и CI
docs/                      # Student-facing documentation content
  index.md                 # Главная страница курса
  learning-format.md       # Описание формата обучения
  course-structure.md      # Таблица курса из 12 модулей
  modules/                 # Страницы модулей для MkDocs
templates/module/          # Шаблоны для новых модулей
.github/workflows/         # GitHub Pages deployment workflow
```

## Локальный запуск documentation site

```bash
pip install -r requirements.txt
mkdocs serve
```

Чтобы проверить production build локально:

```bash
mkdocs build
```

## Добавление или обновление модуля

1. Обновите metadata модуля в `course.yml`.
2. Добавьте или измените student-facing content в `docs/modules/<module-folder>/`.
3. Убедитесь, что в модуле есть `index.md`, `homework.md`, `checklist.md` и `resources.md`.
4. Обновите навигацию в `mkdocs.yml`, если страницы добавлены, удалены или переименованы.
5. Добавляйте external links только в соответствующий `resources.md` модуля.

## Как использовать Codex

Используйте Codex для небольших и удобных для ревью изменений документации, обновлений структуры, навигации и проверок консистентности. Codex должен сохранять русские объяснения, оставлять распространенные English technical terms там, где это естественно, и не делать большие переписывания без явного запроса.

## Student-facing content

Student-facing материалы курса находятся в `docs/`. Старый top-level directory `modules/` больше не используется, потому что MkDocs читает страницы из `docs/`.

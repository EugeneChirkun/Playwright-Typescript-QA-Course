# AGENTS.md

## Project purpose

Этот repository содержит MkDocs-based documentation site для структурированного курса: опытный Manual QA engineer переходит в Automation QA с TypeScript и Playwright. Repository остается documentation-only и не является student automation project.

## Language rules

- Student-facing content must be in `docs/` and should be written in Russian.
- Common technical terms можно оставлять на English: Node.js, npm, VS Code, Git, branch, commit, pull request, TypeScript, Playwright, Page Object Model, fixture, locator, assertion, CI.
- Preserve Russian explanations and common English technical terms.


## Language and terminology

- Use Russian for student-facing explanations.
- Do not mix random English words into Russian sentences.
- Keep official names and common technical terms in English.
- On first mention, optionally use Russian + English in parentheses, for example: "ветка (branch)" or "pull request (PR)".
- Prefer consistency over literal translation.

## Repository structure

- `README.md` — обзор для maintainer/contributor.
- `AGENTS.md` — инструкции для Codex и других coding agents.
- `course.yml` — source of truth для метаданных модулей.
- `mkdocs.yml` — конфигурация MkDocs site и навигация.
- `requirements.txt` — зависимости для локальной сборки и CI.
- `docs/` — материалы сайта для студентов.
- `docs/modules/XX-module-name/` — страницы модулей для MkDocs.
- `templates/module/` — шаблоны для новых модулей.
- `.github/workflows/deploy-docs.yml` — workflow для деплоя GitHub Pages.

## Documentation site rules

- MkDocs читает страницы для студентов из `docs`.
- `mkdocs.yml` нужно обновлять при изменении навигации.
- Используйте относительные ссылки, совместимые с MkDocs.
- Не добавляйте файлы Playwright project, `package.json`, test scripts или scaffolding для automation project.

## Формат файлов модуля

Каждая папка модуля в `docs/modules/` должна содержать:

- `index.md` — обзор модуля с целью, важностью для QA, темами, теорией, примерами кода, QA-сценариями, практикой, вопросами и итогами.
- `homework.md` — задания, ожидаемый результат, способ отправки и чеклист для проверки.
- `checklist.md` — Markdown-чекбоксы.
- `resources.md` — официальная документация, дополнительные материалы, видео и инструменты.

## Writing style

- Пишите дружелюбно, структурно и коротко.
- Не делайте большие переписывания без явного запроса.
- Пока модуль находится в `draft` или `planned`, не добавляйте длинную теорию без отдельного запроса.
- Делайте заготовки осмысленными, чтобы их было легко расширять.

## Link/resource rules

- Добавляйте внешние ссылки только в соответствующий `resources.md` модуля.
- Отдавайте предпочтение официальной документации.
- Не добавляйте случайные низкокачественные tutorials.
- Для будущих материалов используйте `TODO`-заготовки.

## Change policy

- `course.yml` is the source of truth для метаданных модулей.
- Синхронизируйте `course.yml`, `docs/modules/` и навигацию в `mkdocs.yml`.
- Сохраняйте существующий контент, когда перемещаете или адаптируете файлы.
- Делайте изменения небольшими и удобными для ревью.

## Definition of done

- Необходимые файлы созданы, перемещены или обновлены.
- Материалы для студентов находятся в `docs/`.
- Пути в `course.yml` указывают на `docs/modules/...`.
- Навигация в `mkdocs.yml` соответствует структуре документации.
- MkDocs build запускается, когда зависимости доступны.
- Git status показывает только ожидаемые изменения перед commit.

## Final response format after changes

After changes, summarize:

- Files created.
- Files moved.
- Files updated.
- Whether old `modules/` was removed.
- Whether `mkdocs build` passed.
- Assumptions and recommended next step.

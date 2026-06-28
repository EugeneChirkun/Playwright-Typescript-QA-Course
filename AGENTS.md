# AGENTS.md

## Project purpose

Этот repository содержит MkDocs-based documentation site для structured training course: опытный Manual QA engineer переходит в Automation QA с TypeScript и Playwright. Repository остается documentation-only и не является student automation project.

## Language rules

- Student-facing content must be in `docs/` and should be written in Russian.

## Язык и терминология

- Student-facing Markdown must use natural Russian.
- Student-facing documentation must be written in natural Russian. English words may be used only for official names, file names, commands, APIs and established technical terms. Do not write hybrid phrases like `через terminal`, `в project`, `packages устанавливаются`, `practice during the lesson`, `Homework`, `Tasks` or `Checklist`.
- Do not mix random English words into Russian sentences.
- Keep official product names, commands, APIs, file names and common IT terms in English.
- On first mention, optionally use Russian + English in parentheses, for example: `ветка (branch)` or `pull request (PR)`.
- Do not translate headings back into English.
- Do not change code blocks or commands only for language normalization.
- Prefer consistency over literal translation.
- Common technical terms можно оставлять на English: Node.js, npm, npx, VS Code, Git, GitHub, GitHub Actions, branch, commit, pull request, PR, merge, code review, TypeScript, JavaScript, Playwright, API, CI, HTML report, Trace Viewer, Page Object Model, Page Object, fixture, locator, assertion, test runner, package.json, package-lock.json, node_modules, README.md, AGENTS.md, mkdocs.yml, course.yml.

## Repository structure

- `README.md` — maintainer/contributor overview.
- `AGENTS.md` — instructions for Codex and other coding agents.
- `course.yml` — source of truth for module metadata.
- `mkdocs.yml` — MkDocs site configuration and navigation.
- `requirements.txt` — dependencies for local build and CI.
- `docs/` — student-facing documentation site content.
- `docs/modules/XX-module-name/` — страницы модулей для MkDocs.
- `templates/module/` — шаблоны для новых модулей.
- `.github/workflows/deploy-docs.yml` — GitHub Pages deployment workflow.

## Documentation site rules

- MkDocs reads student-facing pages from `docs/`.
- `mkdocs.yml` must be updated when navigation changes.
- Use MkDocs-compatible relative links.
- Do not add Playwright project files, `package.json`, test scripts or automation project scaffolding.

## Module file format

Each module folder under `docs/modules/` must contain:

- `index.md` — обзор модуля с целью, QA-релевантностью, темами, теорией, примерами кода, QA-сценариями, практикой, вопросами и кратким итогом.
- `homework.md` — домашние задания, ожидаемый результат, правила отправки и чеклист для проверки.
- `checklist.md` — Markdown checkboxes.
- `resources.md` — официальная документация, дополнительные материалы, видео и инструменты.

## Writing style

- Пишите дружелюбно, структурно и коротко.
- Не делайте large rewrites unless explicitly asked.
- Пока модуль находится в `draft` или `planned`, не добавляйте длинную теорию без отдельного запроса.
- Делайте placeholders осмысленными, чтобы их было легко расширять.

## Link/resource rules

- Add external links only to the relevant module `resources.md`.
- Prefer official documentation.
- Do not add random low-quality tutorials.
- For future materials use `TODO` placeholders.

## Change policy

- `course.yml` is the source of truth for module metadata.
- Keep `course.yml`, `docs/modules/` and `mkdocs.yml` navigation synchronized.
- Preserve existing content where possible when moving or adapting files.
- Keep changes small and reviewable.

## Definition of done

- Required files are created, moved or updated.
- Student-facing content is under `docs/`.
- `course.yml` paths point to `docs/modules/...`.
- `mkdocs.yml` navigation matches the documentation structure.
- MkDocs build is run when dependencies are available.
- Git status shows only expected changes before commit.

## Final response format after changes

After changes, summarize:

- Files created.
- Files moved.
- Files updated.
- Whether old `modules/` was removed.
- Whether `mkdocs build` passed.
- Assumptions and recommended next step.

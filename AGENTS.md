# AGENTS.md

## Project purpose

Этот repository содержит MkDocs-based documentation site для structured training course: опытный Manual QA engineer переходит в Automation QA с TypeScript и Playwright. Repository остается documentation-only и не является student automation project.

## Language rules

- Student-facing content must be in `docs/` and should be written in Russian.
- Common technical terms можно оставлять на English: Node.js, npm, VS Code, Git, branch, commit, pull request, TypeScript, Playwright, Page Object Model, fixture, locator, assertion, CI.
- Preserve Russian explanations and common English technical terms.

## Repository structure

- `README.md` — maintainer/contributor overview.
- `AGENTS.md` — instructions for Codex and other coding agents.
- `course.yml` — source of truth for module metadata.
- `mkdocs.yml` — MkDocs site configuration and navigation.
- `requirements.txt` — dependencies for local build and CI.
- `docs/` — student-facing documentation site content.
- `docs/modules/XX-module-name/` — module pages for MkDocs.
- `templates/module/` — templates for new modules.
- `.github/workflows/deploy-docs.yml` — GitHub Pages deployment workflow.

## Documentation site rules

- MkDocs reads student-facing pages from `docs/`.
- `mkdocs.yml` must be updated when navigation changes.
- Use MkDocs-compatible relative links.
- Do not add Playwright project files, `package.json`, test scripts or automation project scaffolding.

## Module file format

Each module folder under `docs/modules/` must contain:

- `index.md` — module overview with Goal, QA relevance, Topics, Theory, Code examples, QA use cases, Practice, Questions and Summary.
- `homework.md` — tasks, expected result, submission and review checklist.
- `checklist.md` — Markdown checkboxes.
- `resources.md` — official documentation, extra reading, videos and tools.

## Writing style

- Пишите дружелюбно, структурно и коротко.
- Не делайте large rewrites unless explicitly asked.
- Пока module находится в `draft` или `planned`, не добавляйте длинную theory без отдельного запроса.
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

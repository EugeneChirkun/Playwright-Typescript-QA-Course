# Playwright TypeScript QA Course

## Repository purpose

This repository contains the documentation source for a Russian-language course that helps experienced Manual QA engineers move to Automation QA with TypeScript and Playwright.

## GitHub Pages site

TODO: add GitHub Pages URL after the first deployment.

## Target audience

Experienced Manual QA engineers who understand QA processes and want to learn automation with TypeScript, Playwright, Git, CI and practical test project structure.

## Expected result after 3 months

After 3 months, students should be able to create a basic Playwright project, write UI and API tests, use Page Object Model, fixtures, test data, Git branches, commits and pull requests.

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
  course-structure.md      # 12-module course table
  modules/                 # Module pages used by MkDocs
templates/module/          # Templates for new modules
.github/workflows/         # GitHub Pages deployment workflow
```

## Run documentation site locally

```bash
pip install -r requirements.txt
mkdocs serve
```

To verify the production build locally:

```bash
mkdocs build
```

## Add or update a module

1. Update module metadata in `course.yml`.
2. Add or edit student-facing content under `docs/modules/<module-folder>/`.
3. Ensure the module has `index.md`, `homework.md`, `checklist.md` and `resources.md`.
4. Update `mkdocs.yml` navigation when pages are added, removed or renamed.
5. Keep external links in the relevant module `resources.md`.

## How Codex should be used

Use Codex for small, reviewable documentation changes, structure updates, navigation updates and consistency checks. Codex should preserve Russian explanations, keep common English technical terms where natural, and avoid large rewrites unless explicitly requested.

## Student-facing content

Student-facing course content is located in `docs/`. The old top-level `modules/` directory is no longer used because MkDocs reads pages from `docs/`.

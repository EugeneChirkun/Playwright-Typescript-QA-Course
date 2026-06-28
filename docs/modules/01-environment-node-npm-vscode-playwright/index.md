# Модуль 01: Окружение, Node.js, npm, VS Code и Playwright

## Цель модуля

После этого модуля студент сможет подготовить рабочее окружение Automation QA, установить и проверить Node.js и npm, создать первый Playwright-проект, запустить тесты, открыть HTML report и добавить базовые npm scripts для ежедневной работы.

## Почему это важно для Automation QA

Automation QA — это не только написание тестов. Инженер также работает с инструментами, структурой проекта, зависимостями, командами запуска, настройками раннера тестов и отчетами. Если окружение настроено неправильно, тесты могут не запускаться, вести себя нестабильно или давать непонятные ошибки.

## Темы модуля

- Рабочее окружение Automation QA.
- Node.js и запуск JavaScript/TypeScript вне браузера.
- npm, зависимости, `package.json`, `package-lock.json`, `node_modules`, `npx`.
- VS Code как основной редактор для проекта автоматизации.
- Установка Playwright через терминал и VS Code extension.
- Первый запуск тестов Playwright и открытие HTML report.
- Разбор первого простого теста.
- Базовые npm scripts для удобного запуска команд.

## 1. Что такое рабочее окружение Automation QA

Рабочее окружение Automation QA — это набор инструментов, которые нужны, чтобы писать, запускать и поддерживать автоматизированные тесты. Обычно в него входят среда выполнения, менеджер пакетов, редактор кода, раннер тестов, браузеры, терминал, Git и инструменты для отчетов.

Для курса нам нужны Node.js, npm, VS Code и Playwright. Эти инструменты позволяют создать тестовый проект, установить зависимости, запускать тесты из терминала и анализировать результаты через HTML report.

## 2. Node.js

Node.js — это среда выполнения JavaScript вне браузера. Благодаря Node.js код на JavaScript и TypeScript может запускаться на компьютере разработчика или в CI, а не только внутри Chrome, Firefox или Safari.

Playwright использует Node.js, потому что сам раннер тестов, CLI-команды, конфигурация и npm-пакеты работают в Node.js-окружении. Для обучения и реальных проектов лучше использовать LTS-версию Node.js: она стабильнее и дольше поддерживается.

Проверить установку можно командами:

```bash
node -v
npm -v
```

Если обе команды показывают версии, значит Node.js и npm доступны в терминале.

## 3. npm

npm — это менеджер пакетов для экосистемы JavaScript/TypeScript. Он помогает устанавливать зависимости, запускать scripts и хранить информацию о проекте.

Основные понятия:

- `dependencies` — пакеты, которые нужны приложению во время работы.
- `devDependencies` — пакеты, которые нужны для разработки, тестирования и сборки. Playwright обычно находится здесь, потому что нужен для тестов.
- `package.json` — главный файл с названием проекта, зависимостями и scripts.
- `package-lock.json` — файл, который фиксирует точные версии установленных dependencies, чтобы у всех участников команды было одинаковое окружение.
- `node_modules` — папка, куда npm устанавливает пакеты. Ее не коммитят в Git, потому что она большая и может быть восстановлена через `npm install` или `npm ci`.
- `npm install` — устанавливает зависимости и может обновлять `package-lock.json`.
- `npm ci` — устанавливает зависимости строго по `package-lock.json`; часто используется в CI.
- `npm run` — запускает scripts из `package.json`.
- `npx` — запускает CLI-команду из установленного пакета без ручного поиска пути к файлу.

Примеры команд:

```bash
npm install
npm ci
npm run test
npx playwright test
```

Пример части `package.json`:

```json
{
  "devDependencies": {
    "@playwright/test": "latest"
  },
  "scripts": {
    "test": "playwright test"
  }
}
```

## 4. VS Code

VS Code — удобный редактор для Automation QA. В нем можно писать тесты, открывать структуру проекта, искать по файлам, запускать команды во встроенном терминале и читать ошибки прямо рядом с кодом.

Полезные возможности VS Code:

- File Explorer помогает быстро ориентироваться в структуре проекта.
- Search помогает находить тесты, locators, fixtures и настройки.
- Встроенный терминал позволяет запускать `npm`, `npx` и команды Playwright без отдельного окна.
- Extensions добавляют подсказки, форматирование, анализ кода и удобный запуск тестов.

Рекомендуемые расширения:

- Playwright Test for VS Code.
- ESLint.
- Prettier.
- GitLens.
- DotENV.

## 5. Установка Playwright

Playwright можно установить двумя способами.

Первый вариант — через терминал:

```bash
npm init playwright@latest
```

Второй вариант — через Playwright Test for VS Code extension. Extension может помочь создать проект и запускать тесты через интерфейс VS Code.

Во время установки Playwright обычно задает несколько вопросов:

- TypeScript или JavaScript — для курса выбираем TypeScript.
- Папка для тестов — обычно `tests`.
- Добавлять GitHub Actions — можно выбрать `yes`, если учебный проект будет использовать CI, или `no`, если пока нужен только локальный проект.
- Установить браузеры — выбираем `yes`, чтобы Playwright сразу скачал браузеры.

Важно: эти команды нужно выполнять в отдельном учебном проекте, а не в этом репозитории курса.

## 6. Структура проекта после установки

После установки Playwright-проект обычно содержит:

```text
tests/
playwright.config.ts
package.json
package-lock.json
node_modules/
```

Что это значит:

- `tests/` — папка с тестами.
- `playwright.config.ts` — настройки раннера тестов Playwright: браузеры, timeout, retries, reporter и другие параметры.
- `package.json` — зависимости и npm scripts проекта.
- `package-lock.json` — точные версии dependencies.
- `node_modules/` — установленные пакеты; эту папку не нужно коммитить.

## 7. Первый запуск тестов

Основные команды Playwright:

```bash
npx playwright test
npx playwright test --headed
npx playwright test --ui
npx playwright show-report
```

Что делает каждая команда:

- `npx playwright test` запускает все тесты в headless-режиме.
- `npx playwright test --headed` запускает тесты с видимым окном браузера.
- `npx playwright test --ui` открывает Playwright UI mode для удобного запуска и отладки тестов.
- `npx playwright show-report` открывает HTML report после выполнения тестов.

## 8. Разбор первого теста

Пример первого теста:

```ts
import { test, expect } from '@playwright/test';

test('Playwright homepage has correct title', async ({ page }) => {
  await page.goto('https://playwright.dev/');

  await expect(page).toHaveTitle(/Playwright/);
});
```

Разбор по строкам:

- `import { test, expect } from '@playwright/test';` подключает функции раннера тестов Playwright.
- `test(...)` объявляет тест и его название.
- `async` показывает, что внутри теста будут асинхронные действия.
- `page` — объект страницы браузера, через который тест открывает страницы и взаимодействует с UI.
- `page.goto(...)` открывает указанный URL.
- `expect(...)` создает проверку результата.
- `toHaveTitle(/Playwright/)` — assertion, который проверяет title страницы.
- `await` ждет завершения Playwright-действия или проверки.

В этом модуле не нужно глубоко разбирать `async` и `await`. Эта тема будет подробно объяснена позже в отдельном модуле.

## 9. npm scripts

Чтобы не писать длинные команды каждый раз, их добавляют в `package.json` в раздел `scripts`:

```json
{
  "scripts": {
    "test": "playwright test",
    "test:headed": "playwright test --headed",
    "test:ui": "playwright test --ui",
    "report": "playwright show-report"
  }
}
```

После этого можно запускать команды так:

```bash
npm run test
npm run test:headed
npm run test:ui
npm run report
```

npm scripts полезны, потому что команда становится короче, понятнее и одинаковой для всей команды.

## Практика на занятии

1. Проверить версии Node.js и npm.
2. Создать отдельный Playwright-проект для практики.
3. Запустить стандартные тесты.
4. Открыть HTML report.
5. Создать новый простой тест.
6. Добавить npm scripts в `package.json`.
7. Запустить тесты через npm scripts.

## Вопросы для проверки понимания

1. Что такое Node.js?
2. Почему Playwright нужен Node.js?
3. Почему лучше использовать LTS-версию Node.js?
4. Что такое npm?
5. Что хранится в `package.json`?
6. Для чего нужен `package-lock.json`?
7. Почему `node_modules` не нужно коммитить в Git?
8. Чем `npm install` отличается от `npm ci` на базовом уровне?
9. Что делает `npm run`?
10. Что такое `npx`?
11. Для чего Automation QA использует VS Code?
12. Как запустить тесты Playwright из терминала?
13. Для чего нужен `playwright.config.ts`?
14. Что такое assertion?
15. Почему в Playwright-командах часто используется `await`?

## Краткий итог

В этом модуле студент настраивает базовое окружение Automation QA, понимает роль Node.js, npm, VS Code и Playwright, создает первый Playwright-проект, запускает тесты, открывает HTML report и добавляет npm scripts для удобной ежедневной работы.

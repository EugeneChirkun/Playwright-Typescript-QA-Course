# Module 01: Environment, Node.js, npm, VS Code and Playwright

## Цель модуля

После этого модуля вы сможете подготовить базовое рабочее окружение Automation QA: установить и проверить Node.js и npm, создать первый Playwright project, запустить тесты, открыть HTML report и добавить удобные npm scripts для ежедневной работы.

Также вы поймете, зачем Automation QA нужны инструменты вокруг тестов: зависимости, структура project, `package.json`, `node_modules`, terminal, VS Code и test runner.

## Почему это важно для Automation QA

Automation QA — это не только написание тестов. В реальной работе нужно уверенно пользоваться инструментами, понимать структуру project, устанавливать dependencies, запускать команды, читать ошибки, настраивать scripts и анализировать reports.

Если окружение настроено неправильно, тесты могут не запускаться даже при корректном коде. Поэтому первый шаг в автоматизации — понять, из каких частей состоит рабочая среда и как они связаны между собой.

## Темы модуля

- Что входит в рабочее окружение Automation QA.
- Что такое Node.js и зачем он нужен для Playwright.
- Что такое npm, dependencies и scripts.
- Зачем нужны `package.json`, `package-lock.json` и `node_modules`.
- Чем отличаются `npm install`, `npm ci`, `npm run` и `npx`.
- Как VS Code помогает писать и запускать автотесты.
- Какие VS Code extensions полезны для Automation QA.
- Как установить Playwright и создать первый project.
- Как запустить тесты в Playwright.
- Как открыть Playwright HTML report.
- Как прочитать первый простой Playwright test.
- Как добавить базовые npm scripts.

## 1. Что такое рабочее окружение Automation QA

Рабочее окружение Automation QA — это набор tools, которые нужны, чтобы писать, запускать и поддерживать automated tests.

Обычно окружение включает:

- Node.js для запуска JavaScript и TypeScript tools вне browser.
- npm для установки packages и запуска scripts.
- VS Code как code editor.
- Playwright как framework и test runner.
- Browser binaries, которые Playwright использует для запуска tests.
- Git и GitHub для хранения code и совместной работы.
- Terminal для выполнения commands.

Для Manual QA это похоже на набор инструментов для тестирования: browser, DevTools, test management system, баг-трекер и тестовая документация. В Automation QA добавляются code editor, dependencies, command line и reports.

## 2. Node.js

Node.js — это среда выполнения JavaScript вне browser. Обычно JavaScript выполняется внутри browser, например на странице сайта. Node.js позволяет запускать JavaScript и tools на вашем компьютере через terminal.

TypeScript сам по себе тоже не запускается напрямую в browser или terminal как обычный готовый код. В project используются tools, которые компилируют или обрабатывают TypeScript. Эти tools работают через Node.js.

Playwright для TypeScript использует Node.js, потому что:

- Playwright test runner запускается в Node.js.
- npm packages устанавливаются и управляются через Node.js ecosystem.
- команды Playwright выполняются из terminal.
- tests, config и reports обрабатываются инструментами Node.js.

Для обучения и реальной работы рекомендуется LTS version Node.js. LTS означает Long Term Support: такая версия стабильнее, дольше поддерживается и обычно лучше подходит для production и командной разработки.

Проверить установку Node.js и npm можно командами:

```bash
node -v
npm -v
```

Если обе команды показывают version numbers, значит Node.js и npm доступны в terminal.

## 3. npm

npm — это package manager для Node.js. Он помогает устанавливать packages, управлять dependencies и запускать scripts из `package.json`.

Package — это готовая библиотека или tool. Например, Playwright устанавливается как npm package.

Dependency — это package, который нужен project. Dependencies бывают двух основных типов:

- `dependencies` — packages, которые нужны приложению во время работы.
- `devDependencies` — packages, которые нужны для разработки, тестирования, сборки или проверки code.

В automation project Playwright обычно находится в `devDependencies`, потому что он нужен для разработки и запуска tests, а не для работы пользовательского приложения.

### package.json

`package.json` — главный файл Node.js project. В нем описаны project name, scripts, dependencies и devDependencies.

Пример scripts в `package.json`:

```json
{
  "scripts": {
    "test": "playwright test"
  }
}
```

После этого команду можно запускать так:

```bash
npm run test
```

### package-lock.json

`package-lock.json` фиксирует точные versions установленных packages и их внутренних dependencies. Это помогает всей команде устанавливать одинаковые versions и получать более предсказуемый результат.

Обычно `package-lock.json` нужно commit в repository.

### node_modules

`node_modules` — это папка, куда npm устанавливает packages. Она может быть очень большой, потому что содержит все dependencies project.

`node_modules` не нужно commit в Git. Вместо этого в repository хранят `package.json` и `package-lock.json`, а каждый developer или CI server восстанавливает dependencies командой `npm install` или `npm ci`.

### npm install

`npm install` устанавливает dependencies из `package.json` и обновляет `package-lock.json`, если это необходимо.

```bash
npm install
```

Эта команда часто используется локально во время разработки.

### npm ci

`npm ci` устанавливает dependencies строго по `package-lock.json`. Команда обычно используется в CI, потому что дает более чистую и воспроизводимую установку.

```bash
npm ci
```

### npm run

`npm run` запускает script из `package.json`.

```bash
npm run test
```

Это удобно, потому что команда становится короткой и одинаковой для всей команды.

### npx

`npx` запускает package command без необходимости писать полный путь до executable file в `node_modules`.

Например:

```bash
npx playwright test
```

Эта команда запускает локально установленный Playwright test runner.

## 4. VS Code

VS Code — это code editor, в котором удобно писать, запускать и анализировать automated tests.

Для Automation QA VS Code полезен, потому что в нем есть:

- file explorer для просмотра структуры project;
- встроенный terminal для запуска commands;
- search по files и project;
- подсветка syntax для TypeScript;
- подсказки и автодополнение;
- extensions для Playwright, linting, formatting и Git;
- удобное чтение errors и переход к нужной строке code.

Automation QA часто работает в цикле: изменить test, запустить command, прочитать error, исправить code, снова запустить test. VS Code помогает делать это в одном месте.

Рекомендуемые extensions:

- Playwright Test for VS Code — запуск и debugging Playwright tests из editor.
- ESLint — подсветка code quality issues.
- Prettier — автоматическое форматирование code.
- GitLens — расширенная работа с Git history и authorship.
- DotENV — подсветка `.env` files, которые часто используются для environment variables.

## 5. Installing Playwright

Playwright можно установить двумя способами: через terminal или через VS Code extension.

### Вариант 1. Установка через terminal

В новом учебном project выполните команду:

```bash
npm init playwright@latest
```

Эта команда создаст Playwright project и предложит несколько setup choices.

Типичные ответы для обучения:

- TypeScript — yes, потому что курс использует TypeScript.
- tests folder — можно оставить `tests`.
- GitHub Actions — yes, если хотите сразу увидеть пример CI setup; no, если пока хотите сосредоточиться только на локальном запуске.
- install browsers — yes, чтобы Playwright сразу скачал browsers для тестов.

### Вариант 2. Установка через VS Code extension

Extension Playwright Test for VS Code умеет создавать новый Playwright project через UI. Это удобно, если вы только привыкаете к terminal.

Но Automation QA все равно должен понимать terminal commands, потому что они используются в CI, documentation, onboarding и командной работе.

Важно: в этом repository мы не создаем student automation project. Playwright project нужно создавать отдельно, например в отдельной папке на вашем компьютере.

## 6. Project structure after installation

После установки Playwright project обычно содержит такие files и folders:

```text
tests/
playwright.config.ts
package.json
package-lock.json
node_modules/
```

Что это значит:

- `tests/` — папка с test files.
- `playwright.config.ts` — configuration file Playwright: browsers, retries, reports, baseURL и другие settings.
- `package.json` — scripts и dependencies project.
- `package-lock.json` — зафиксированные exact versions dependencies.
- `node_modules/` — установленные packages, которые не нужно commit в Git.

## 7. First test run

Основные команды для первого запуска:

```bash
npx playwright test
npx playwright test --headed
npx playwright test --ui
npx playwright show-report
```

Что делает каждая команда:

- `npx playwright test` запускает все tests в headless mode, то есть browsers открываются без видимого окна.
- `npx playwright test --headed` запускает tests с видимым browser window. Это полезно для обучения и debugging.
- `npx playwright test --ui` открывает Playwright UI mode, где можно выбирать tests, смотреть steps и быстрее разбираться в поведении.
- `npx playwright show-report` открывает HTML report после запуска tests.

HTML report помогает увидеть, какие tests прошли, какие упали, сколько времени занял запуск и где произошла ошибка.

## 8. First test explanation

Пример первого test:

```ts
import { test, expect } from '@playwright/test';

test('Playwright homepage has correct title', async ({ page }) => {
  await page.goto('https://playwright.dev/');

  await expect(page).toHaveTitle(/Playwright/);
});
```

Разберем по строкам:

- `import { test, expect } from '@playwright/test';` подключает из Playwright две главные функции: `test` для описания test case и `expect` для проверки результата.
- `test('Playwright homepage has correct title', ...)` создает test с понятным названием.
- `async` показывает, что внутри test будут asynchronous operations: browser actions, navigation и checks.
- `({ page })` — это Playwright fixture. `page` представляет browser tab, с которым взаимодействует test.
- `await page.goto('https://playwright.dev/');` открывает страницу Playwright website.
- `expect(page).toHaveTitle(/Playwright/)` проверяет, что title страницы содержит слово `Playwright`.
- Такая проверка называется assertion: она сравнивает ожидаемый результат с фактическим.
- `await` говорит test runner подождать завершения действия или проверки перед переходом к следующей строке.

Пока не нужно глубоко разбирать `async` и `await`. В этом модуле важно понять общий смысл: Playwright выполняет действия в browser не мгновенно, поэтому test должен дожидаться результата. Подробно asynchronous code будет разобран в отдельном модуле.

## 9. npm scripts

В `package.json` можно добавить scripts для частых commands:

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

После этого commands будут выглядеть так:

```bash
npm run test
npm run test:headed
npm run test:ui
npm run report
```

Scripts полезны, потому что:

- делают commands короче;
- стандартизируют запуск tests для всей команды;
- уменьшают риск опечаток;
- помогают новичкам быстрее понять, как работать с project;
- используются в CI pipelines.

## Практика на занятии

1. Проверить Node.js и npm versions.
2. Создать отдельный Playwright project вне этого documentation repository.
3. Запустить default tests.
4. Открыть HTML report.
5. Создать новый простой test для Playwright homepage.
6. Добавить npm scripts в `package.json`.
7. Запустить tests через npm scripts.

## Вопросы для проверки понимания

- Что такое Node.js?
- Почему Playwright нужен Node.js?
- Что такое npm?
- Что такое package manager?
- Что такое `package.json`?
- Для чего нужен `package-lock.json`?
- Почему `node_modules` не нужно commit в Git?
- Чем `npm install` отличается от `npm ci` на базовом уровне?
- Что делает `npm run`?
- Что такое `npx`?
- Как запустить Playwright tests из terminal?
- Для чего нужен `playwright.config.ts`?
- Что такое assertion?
- Почему Playwright commands часто используют `await`?
- Чем полезен HTML report для Automation QA?

## Краткий итог

В этом модуле вы подготовили базу для дальнейшего изучения Playwright и TypeScript. Теперь вы знаете, из каких инструментов состоит рабочее окружение Automation QA, зачем нужны Node.js и npm, как устроены основные project files, как создать Playwright project, запустить первый test и открыть HTML report.

Следующий важный шаг — продолжить практику: чаще запускать commands из terminal, читать output и постепенно привыкать к структуре automation project.

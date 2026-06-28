# Домашнее задание: Модуль 01

## Задание 1. Установить Node.js и проверить npm

Установите LTS-версию Node.js с официального сайта. После установки проверьте, что Node.js и npm доступны в терминале:

```bash
node -v
npm -v
```

Сохраните вывод команд одним из способов:

- позже скопируйте его в `README.md` учебного проекта;
- или сделайте скриншот терминала.

## Задание 2. Установить и открыть VS Code

Установите VS Code, если он ещё не установлен, и откройте его.

Проверьте базовые элементы интерфейса:

- найдите, где отображается дерево файлов;
- откройте встроенный терминал через меню `Terminal → New Terminal`.

На этом этапе не нужно глубоко настраивать VS Code. Важно убедиться, что редактор установлен, открывается и в нём доступен терминал.

## Задание 3. Установить расширение Playwright Test for VS Code

В VS Code откройте раздел Extensions, найдите `Playwright Test for VS Code` и установите это расширение VS Code.

Позже оно позволит запускать тесты из панели Testing прямо в редакторе.

## Задание 4. Создать отдельный учебный проект Playwright

Вся практическая работа выполняется в отдельном локальном учебном проекте на вашем компьютере.

Создайте папку проекта и установите Playwright:

```bash
mkdir playwright-ts-training
cd playwright-ts-training
npm init playwright@latest
```

Во время настройки:

- выберите TypeScript;
- используйте папку `tests`;
- установите браузеры, если установщик задаст такой вопрос.

## Задание 5. Открыть учебный проект в VS Code

Откройте созданную папку `playwright-ts-training` в VS Code.

Проверьте, что в проекте видны:

- `package.json`;
- `playwright.config.ts`;
- `tests/`.

Если эти файлы не видны, скорее всего, открыта не та папка. Проверьте, что в VS Code открыта именно папка `playwright-ts-training`, а не папка уровнем выше.

## Задание 6. Запустить стандартные тесты из терминала VS Code

Во встроенном терминале VS Code выполните команды:

```bash
npx playwright test
npx playwright show-report
```

Тесты должны выполниться успешно, а HTML report должен открыться в браузере.

## Задание 7. Проверить запуск тестов из VS Code

Откройте панель Testing в VS Code и найдите Playwright-тесты.

Запустите один тест из редактора. После проверки сделайте скриншот или напишите короткую заметку о том, что тест был виден в VS Code и запускался из панели Testing.

## Задание 8. Создать первый собственный тест

Создайте файл:

```text
tests/homepage.spec.ts
```

Добавьте тест:

```ts
import { test, expect } from '@playwright/test';

test('Playwright homepage has correct title', async ({ page }) => {
  await page.goto('https://playwright.dev/');

  await expect(page).toHaveTitle(/Playwright/);
});
```

Запустите тесты ещё раз и проверьте, что новый тест проходит успешно.

## Задание 9. Добавить npm-скрипты

В `package.json` добавьте скрипты:

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

После этого проверьте команды:

```bash
npm run test
npm run report
```

npm-скрипты делают команды короче и помогают запускать их одинаково каждый раз.

## Задание 10. Подготовить короткий README.md в учебном проекте

Создайте или обновите `README.md` в учебном проекте.

Добавьте разделы:

- требования: версии Node.js и npm, которые использовались;
- установка зависимостей;
- запуск тестов;
- открытие HTML report;
- запуск тестов из VS Code.

Пример структуры:

```md
# Playwright TypeScript Training

## Requirements

- Node.js: ...
- npm: ...

## Install dependencies

npm install

## Run tests

npm run test

## Open report

npm run report

## VS Code

Tests can also be run from the Testing panel after installing the Playwright Test for VS Code extension.
```

## Ожидаемый результат

В учебном проекте должны быть:

- созданный учебный проект Playwright;
- папка `tests/`;
- файл `tests/homepage.spec.ts`;
- файл `playwright.config.ts`;
- файл `package.json` с npm-скриптами;
- файл `package-lock.json`;
- файл `README.md` с краткими инструкциями;
- успешный запуск тестов;
- работающий HTML report;
- установленное расширение Playwright Test for VS Code;
- подтверждение, что тесты видны в панели Testing в VS Code.

Папку `node_modules` не нужно отправлять на проверку и не нужно добавлять в commit в Git.

## Как подготовить результат для проверки

1. Выполните задания в локальном учебном проекте.
2. Проверьте, что тесты запускаются успешно.
3. Проверьте, что HTML report открывается.
4. Подготовьте краткое описание результата:
   - какие команды запускались;
   - прошли ли тесты;
   - были ли ошибки или вопросы;
   - скриншоты или вывод терминала, если это полезно для проверки.

Подробный рабочий процесс с Git будет разобран в модуле 02.

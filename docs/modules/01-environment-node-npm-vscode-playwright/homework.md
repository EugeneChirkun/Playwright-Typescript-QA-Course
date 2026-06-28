# Домашнее задание: Модуль 01

## Задание 1. Проверить окружение

Выполните команды:

```bash
node -v
npm -v
```

Сохраните вывод команд: скопируйте его в README.md вашего student project или приложите screenshot terminal.

## Задание 2. Создать Playwright project

Создайте отдельный учебный project вне repository с материалами курса:

```bash
mkdir playwright-ts-training
cd playwright-ts-training
npm init playwright@latest
```

Во время setup выберите TypeScript и папку `тесты`. Если installer предложит установить browsers, выберите yes.

## Задание 3. Запустить тесты

Внутри созданного project выполните:

```bash
npx playwright test
npx playwright show-report
```

Убедитесь, что тесты запускаются, а HTML report открывается в browser.

## Задание 4. Создать первый собственный тест

Создайте файл:

```text
tests/homepage.spec.ts
```

Добавьте test:

```ts
import { test, expect } from '@playwright/test';

test('Playwright homepage has correct title', async ({ page }) => {
  await page.goto('https://playwright.dev/');

  await expect(page).toHaveTitle(/Playwright/);
});
```

Запустите тесты еще раз и проверьте, что новый test выполняется успешно.

## Задание 5. Добавить npm scripts

В `package.json` добавьте scripts для запуска тесты и report:

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

## Задание 6. Подготовить короткий README.md в student project

Создайте или обновите `README.md` в вашем student project. Добавьте разделы:

- requirements: какие versions Node.js и npm использовались;
- install dependencies: как установить dependencies;
- run tests: как запустить тесты;
- open report: как открыть HTML report.

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
```

## Ожидаемый результат

В student project должны быть:

- созданный Playwright project;
- папка `тесты/`;
- файл `тесты/homepage.spec.ts`;
- файл `playwright.config.ts`;
- файл `package.json` с npm scripts;
- файл `package-lock.json`;
- файл `README.md` с краткой инструкцией;
- успешный запуск тестов;
- открывающийся HTML report.

Папку `node_modules` не нужно отправлять на ревью и не нужно commit в Git.

## Как отправить на проверку

1. Создайте repository на GitHub для student project.
2. Сделайте commit с выполненным домашним заданием.
3. Отправьте changes на GitHub.
4. Создайте pull request или отправьте link на repository, если такой формат согласован с ментором.
5. В описании pull request укажите:
   - какие команды запускались;
   - прошли ли тесты;
   - были ли ошибки или вопросы;
   - screenshots или terminal output, если это нужно для ревью.

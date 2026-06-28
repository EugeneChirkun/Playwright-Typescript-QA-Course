# Домашнее задание: Модуль 01

## Задание 1. Проверить окружение

Выполните команды:

```bash
node -v
npm -v
```

Сохраните вывод команд: скопируйте его в README.md вашего учебного проекта или приложите скриншот терминала.

## Задание 2. Создать проект Playwright

Создайте отдельный учебный проект вне репозитория с материалами курса:

```bash
mkdir playwright-ts-training
cd playwright-ts-training
npm init playwright@latest
```

Во время настройки выберите TypeScript и папку `tests`. Если установщик предложит установить браузеры, выберите yes.

## Задание 3. Запустить тесты

Внутри созданного проекта выполните:

```bash
npx playwright test
npx playwright show-report
```

Убедитесь, что тесты запускаются, а HTML report открывается в браузере.

## Задание 4. Создать первый собственный тест

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

Запустите тесты еще раз и проверьте, что новый тест выполняется успешно.

## Задание 5. Добавить npm-скрипты

В `package.json` добавьте скрипты для запуска тестов и открытия отчёта:

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

## Задание 6. Подготовить короткий README.md в учебном проекте

Создайте или обновите `README.md` в вашем учебном проекте. Добавьте разделы:

- требования: какие версии Node.js и npm использовались;
- установка зависимостей: как установить зависимости;
- запуск тестов: как запустить тесты;
- отчёт: как открыть HTML report.

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

В учебном проекте должны быть:

- созданный проект Playwright;
- папка `tests/`;
- файл `tests/homepage.spec.ts`;
- файл `playwright.config.ts`;
- файл `package.json` с npm-скриптами;
- файл `package-lock.json`;
- файл `README.md` с краткой инструкцией;
- успешный запуск тестов;
- открывающийся HTML report.

Папку `node_modules` не нужно отправлять на ревью и не нужно добавлять в commit в Git.

## Как отправить на проверку

1. Создайте репозиторий на GitHub для учебного проекта.
2. Сделайте commit с выполненным домашним заданием.
3. Отправьте изменения на GitHub.
4. Создайте pull request или отправьте ссылку на репозиторий, если такой формат согласован с ментором.
5. В описании pull request укажите:
   - какие команды запускались;
   - прошли ли тесты;
   - были ли ошибки или вопросы;
   - скриншоты или вывод терминала, если это нужно для ревью.

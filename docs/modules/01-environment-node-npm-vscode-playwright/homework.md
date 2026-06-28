# Домашнее задание: Модуль 01

## Задание 1. Проверить окружение

Выполните команды:

```bash
node -v
npm -v
```

Сохраните вывод команд: скопируйте его в README.md вашего учебного проекта или приложите скриншот терминала.

## Задание 2. Создать проект Playwright

Создайте отдельный учебный проект Playwright на своём компьютере:

```bash
mkdir playwright-ts-training
cd playwright-ts-training
npm init playwright@latest
```

Во время настройки выберите TypeScript и папку `tests`. Если установщик предложит установить браузеры, выберите yes.

## Задание 3. Запустить тесты

В своём локальном проекте выполните:

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

Запустите тесты ещё раз и проверьте, что новый тест выполняется успешно.

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


## Задание 7. Проверить работу в VS Code

1. Установите VS Code, если он ещё не установлен.
2. Установите расширение Playwright Test for VS Code.
3. Откройте папку учебного проекта Playwright в VS Code.
4. Запустите тесты из встроенного терминала.
5. Найдите тесты в панели Testing и запустите один тест из редактора.
6. Сделайте скриншот или добавьте короткую заметку о том, что тест был виден в VS Code и запускался из панели Testing.

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
- открывающийся HTML report;
- установленное расширение Playwright Test for VS Code;
- подтверждение, что тест был виден в панели Testing.

Папку `node_modules` не нужно отправлять на ревью и не нужно добавлять в commit в Git.

## Как подготовить результат для проверки

1. Выполните задания в своём локальном учебном проекте.
2. Проверьте, что тесты запускаются и HTML report открывается.
3. Подготовьте краткое описание результата:
   - какие команды запускались;
   - прошли ли тесты;
   - были ли ошибки или вопросы;
   - скриншоты или вывод терминала, если это нужно для ревью.

Подробный рабочий процесс с Git будет разобран в следующем модуле.

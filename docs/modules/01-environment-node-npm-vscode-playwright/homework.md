# Homework: Module 01

## Задание 1. Проверить окружение

Выполните команды:

```bash
node -v
npm -v
```

Сохраните вывод команд: его можно добавить в README.md учебного проекта, описание PR или приложить скриншотом.

## Задание 2. Создать Playwright project

Создайте отдельную папку для практического проекта. Не создавайте учебный проект внутри репозитория курса.

```bash
mkdir playwright-ts-training
cd playwright-ts-training
npm init playwright@latest
```

Во время установки выберите TypeScript, папку `tests`, установку browsers и базовую конфигурацию Playwright.

## Задание 3. Запустить тесты

Запустите стандартные тесты и откройте HTML report:

```bash
npx playwright test
npx playwright show-report
```

Убедитесь, что тесты завершились успешно или что вы понимаете причину ошибки.

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

Запустите тест и проверьте результат в terminal и HTML report.

## Задание 5. Добавить npm scripts

Добавьте в `package.json` scripts для основных команд:

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

Проверьте запуск:

```bash
npm run test
npm run report
```

## Задание 6. Подготовить короткий README.md в учебном проекте

Добавьте в README.md учебного проекта короткую инструкцию:

- requirements: Node.js LTS и npm;
- как установить dependencies;
- как запустить тесты;
- как открыть HTML report.

## Ожидаемый результат

В учебном проекте должны быть:

- созданный Playwright project;
- папка `tests/`;
- файл `tests/homepage.spec.ts` с первым собственным тестом;
- `package.json` с npm scripts;
- `package-lock.json`;
- локальная папка `node_modules/`, которая не добавляется в Git;
- короткий README.md с командами запуска;
- успешный запуск `npx playwright test` или понятное описание ошибки, если она возникла.

## Как отправить на проверку

1. Загрузите учебный проект на GitHub.
2. Создайте PR или отправьте ссылку на репозиторий.
3. В описании PR или сообщении укажите:
   - версии Node.js и npm;
   - какие команды запускались;
   - прошли ли тесты;
   - ссылку на скриншот или вывод terminal, если это нужно для проверки.
4. Если возникли ошибки, приложите текст ошибки и кратко опишите, что уже пробовали сделать.

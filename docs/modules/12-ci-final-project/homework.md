# Домашнее задание: Модуль 12

## Цель домашнего задания

Работа выполняется в [репозитории домашних заданий](https://github.com/EugeneChirkun/Pw-Ts-Qa-Hw), а не в репозитории документации курса.

Завершить курс: подготовить финальный проект на Playwright и TypeScript и подключить его к GitHub Actions.

Вам нужно до начала работы обновить ветку модуля 12 из личной ветки `master`, создать или обновить workflow, обеспечить запуск typecheck и тестов, подготовить итоговые UI- и API-примеры, заполнить `homework/module-12-ci-final-project/result.md` и открыть итоговый PR в личную ветку `master`.

Практическая работа выполняется в репозитории домашних заданий, а не в репозитории документации курса.

## Перед началом работы

Практическая работа выполняется в репозитории домашних заданий:

`https://github.com/EugeneChirkun/Pw-Ts-Qa-Hw`

Если это первое практическое задание, сначала склонируйте репозиторий:

```bash
git clone https://github.com/EugeneChirkun/Pw-Ts-Qa-Hw.git
cd Pw-Ts-Qa-Hw
```

Если репозиторий уже есть на компьютере, просто откройте его папку в VS Code и обновите ветки. Все команды выполняйте из корня проекта. Не создавайте новую пустую папку или новый проект для текущего модуля.

После принятия предыдущего PR убедитесь, что ваша личная ветка `student/{student-name-slug}/master` обновлена. Затем обновите ветку текущего модуля из этой личной ветки `master`.

При первом локальном переходе в подготовленные удалённые ветки выполните:

```bash
git fetch origin --prune

git switch --track origin/student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch --track origin/student/{student-name-slug}/module-12-ci-final-project
git merge origin/student/{student-name-slug}/master
```

Если локальные ветки уже существуют, используйте более короткие команды:

```bash
git fetch origin --prune

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-12-ci-final-project
git merge origin/student/{student-name-slug}/master
```

Если ветка модуля не найдена, не создавайте ветку со случайным названием. Обратитесь к преподавателю.

## Задание 2. Проверить scripts в package.json

Откройте `package.json`. В нем должны быть scripts, похожие на следующие:

```json
{
  "scripts": {
    "test": "playwright test",
    "test:headed": "playwright test --headed",
    "test:ui": "playwright test --ui",
    "report": "playwright show-report",
    "typecheck": "tsc --noEmit"
  }
}
```

Не ломайте существующие scripts. Если команды проекта уже имеют другие имена, согласуйте workflow с ними.

## Задание 3. Создать workflow GitHub Actions

Создайте файл:

```text
.github/workflows/playwright.yml
```

Добавьте workflow:

```yaml
name: Playwright Tests

on:
  push:
    branches:
      - master
      - 'student/**'
  pull_request:
    branches:
      - master
      - 'student/**'

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Run TypeScript typecheck
        run: npm run typecheck

      - name: Install Playwright browsers
        run: npx playwright install --with-deps

      - name: Run Playwright tests
        run: npm run test

      - name: Upload Playwright report
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: playwright-report
          path: playwright-report/
          retention-days: 7
```

## Задание 4. Подготовить финальную структуру проекта

Создайте или обновите:

```text
tests/module-12/final-ui.spec.ts
tests/module-12/final-api.spec.ts
homework/module-12-ci-final-project/result.md
```

Переиспользуйте Page Objects из предыдущих модулей, API helpers из модуля 11, тестовые данные из предыдущих модулей и fixtures из модуля 10. Не дублируйте код без необходимости.

## Задание 5. Подготовить итоговый UI-тест

Создайте или обновите один читаемый UI-тест, который:

- использует Playwright;
- использует Page Object, если он уже подходит сценарию;
- выбирает стабильный locator;
- содержит хотя бы один ясный assertion.

Если демонстрационное приложение не поддерживает полный сценарий, используйте простую публичную страницу или учебную страницу из предыдущих модулей. Реальная авторизация не обязательна.

## Задание 6. Подготовить итоговый API-тест

Создайте или обновите один API-тест, который:

- использует fixture `request` из Playwright;
- отправляет GET- или POST-запрос;
- проверяет status code и response body;
- использует типизированные данные ответа или API helper.

Допустимый учебный API:

```text
https://jsonplaceholder.typicode.com
```

## Задание 7. Выполнить локальные проверки

```bash
npm ci
npm run typecheck
npm run test
```

Если тесты падают из-за недоступного внешнего демонстрационного сайта, опишите проблему в `result.md`. TypeScript должен пройти проверку, если в коде нет настоящей ошибки.

## Задание 8. Отправить изменения и проверить CI

```bash
git status
git add .
git commit -m "Complete module 12 CI and final project"
git push
```

Откройте PR со следующими ветками:

```text
base: student/{student-name-slug}/master
compare: student/{student-name-slug}/module-12-ci-final-project
```

Не открывайте PR в `master` репозитория. Домашние задания объединяются в личную основную ветку студента `student/{student-name-slug}/master`.

Проверьте результат GitHub Actions в PR. Если запуск упал, найдите ошибочный step, исправьте причину и снова отправьте изменения.

## Задание 9. Заполнить result.md

В `homework/module-12-ci-final-project/result.md` укажите:

- имя текущей ветки;
- созданные и обновленные файлы;
- выполненные команды;
- результаты `npm ci`, `npm run typecheck` и локального `npm run test`;
- запустился ли GitHub Actions и успешно ли он завершился;
- ссылку на artifact с HTML report или пояснение, если он недоступен;
- короткое объяснение CI, workflow GitHub Actions и artifact;
- краткое описание итоговых UI- и API-тестов;
- возникшие сложности;
- вопросы для итогового ревью.

## Ожидаемый результат

- Практическая работа выполнена в репозитории домашних заданий.
- Использована подготовленная ветка `student/{student-name-slug}/module-12-ci-final-project`.
- Перед началом работы ветка модуля обновлена из личной ветки `student/{student-name-slug}/master`.
- Требуемые файлы модуля созданы или обновлены, а `result.md` заполнен, если он предусмотрен заданием.
- `npm run typecheck` и тесты запущены, если они предусмотрены заданием.
- PR открыт в `student/{student-name-slug}/master`.
- `node_modules`, отчёты, состояние авторизации и реальные учётные данные не добавлены в Git.
- [ ] Ветка модуля 12 обновлена из личной ветки `master` до начала работы.
- [ ] Workflow GitHub Actions создан.
- [ ] Итоговые UI- и API-тесты созданы или обновлены.
- [ ] `homework/module-12-ci-final-project/result.md` заполнен.
- [ ] Локальный typecheck выполнен.
- [ ] Тесты по возможности запущены локально.
- [ ] PR открыт в личную ветку `master`.
- [ ] Результат GitHub Actions проверен.
- [ ] В Git нет `node_modules`, сгенерированных отчетов и состояния авторизации.
- [ ] В коде нет реальных учетных данных.
- [ ] В общих материалах нет настоящих имен студентов.

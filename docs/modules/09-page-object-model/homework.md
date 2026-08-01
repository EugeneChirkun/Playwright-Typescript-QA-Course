# Домашнее задание: Модуль 09

## Цель домашнего задания

Цель — переработать простые тесты Playwright с помощью Page Object Model.

Вам нужно:

- перед началом обновить ветку модуля 09 из личной master-ветки;
- создать Page Object classes в `src/pages/`;
- создать тесты Playwright в `tests/module-09-page-object-model/`;
- заполнить `homework/module-09-page-object-model/result.md`;
- запустить тесты из терминала;
- открыть PR в личную master-ветку.

## Задание 1. Подготовить ветку модуля 09

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-09-page-object-model
git merge origin/student/{student-name-slug}/master
git push
```

## Задание 2. Подготовить проект локально

Установите зависимости:

```bash
npm install
```

Если браузеры еще не установлены, выполните:

```bash
npx playwright install
```

## Задание 3. Создать файлы

Создайте Page Object classes:

```text
src/pages/TodoPage.ts
src/pages/PlaywrightHomePage.ts
```

Создайте тестовые файлы:

```text
tests/module-09-page-object-model/todo-page-object.spec.ts
tests/module-09-page-object-model/playwright-home-page-object.spec.ts
```

Создайте файл с результатом:

```text
homework/module-09-page-object-model/result.md
```

## Задание 4. Создать `TodoPage`

В `src/pages/TodoPage.ts` создайте class `TodoPage`.

Требования:

- импортируйте `Page`, `Locator` и `expect` из `@playwright/test`;
- передайте `private page: Page` через constructor;
- создайте locator поля для новой задачи;
- создайте method `open(): Promise<void>`;
- создайте method `addTodo(title: string): Promise<void>`;
- создайте method `expectTodoVisible(title: string): Promise<void>`;
- по возможности создайте method `expectTodoCount(count: number): Promise<void>`.

Рекомендуемая страница:

```text
https://demo.playwright.dev/todomvc/
```

## Задание 5. Создать тест для `TodoPage`

В `tests/module-09-page-object-model/todo-page-object.spec.ts` создайте тесты, которые:

- создают `const todoPage = new TodoPage(page)`;
- открывают страницу TodoMVC;
- добавляют одну задачу и проверяют, что она видима;
- добавляют две задачи и проверяют, что обе видимы.

Тест должен легко читаться. Не дублируйте в нем locators из Page Object.

## Задание 6. Создать `PlaywrightHomePage`

В `src/pages/PlaywrightHomePage.ts` создайте class `PlaywrightHomePage`.

Требования:

- передайте `private page: Page` через constructor;
- создайте method `open(): Promise<void>`;
- создайте method `clickGetStarted(): Promise<void>`;
- создайте method `expectHomePageOpened(): Promise<void>`;
- создайте method `expectInstallationPageOpened(): Promise<void>`.

Используйте страницу:

```text
https://playwright.dev/
```

## Задание 7. Создать тест для `PlaywrightHomePage`

В `tests/module-09-page-object-model/playwright-home-page-object.spec.ts` создайте тест, который:

- открывает главную страницу Playwright через Page Object;
- проверяет главную страницу;
- нажимает `Get started`;
- проверяет, что открылась страница Installation.

Используйте methods Page Object и не дублируйте его locators в тесте.

## Задание 8. Проверить чистоту кода

Перед запуском тестов убедитесь, что:

- названия methods понятны;
- Page Object не скрывает весь сценарий в одном method;
- нет названий `doEverything` и `click1`;
- locators не дублируются в тестах;
- нет огромного class для несвязанных страниц;
- сформированные отчеты не подготовлены к commit.

## Задание 9. Запустить тесты

```bash
npm run test
```

При необходимости откройте отчет:

```bash
npm run report
```

## Задание 10. Заполнить `result.md`

В `homework/module-09-page-object-model/result.md` укажите:

- название текущей ветки;
- созданные файлы;
- использованные команды;
- прошла ли команда `npm run test`;
- краткое объяснение Page Object Model;
- почему locators были перенесены в Page Object;
- как работает constructor с `Page`;
- что такое methods действий и methods с assertions;
- что стало проще после переработки;
- что оказалось сложнее;
- оставшиеся вопросы или проблемы.

## Задание 11. Сделать commit, push и PR

Перед commit еще раз убедитесь, что тесты проходят:

```bash
npm run test
```

Затем выполните:

```bash
git status
git add .
git commit -m "Complete module 09 page object model homework"
git push
```

Направление PR:

```text
base: student/{student-name-slug}/master
compare: student/{student-name-slug}/module-09-page-object-model
```

## Ожидаемый результат

- Ветка модуля 09 обновлена из личной master-ветки до начала работы.
- Page Object classes созданы в `src/pages/`.
- Тесты Playwright созданы в `tests/module-09-page-object-model/`.
- Файл `result.md` заполнен.
- Команда `npm run test` проходит успешно.
- Сформированные отчеты не добавлены в Git.
- В тестах нет дублирования locators.
- Изменения сохранены в commit и отправлены в удаленный репозиторий.
- PR открыт в личную master-ветку.
- `node_modules` не добавлен в Git.
- В коде и документации нет настоящих имен учащихся.

# Домашнее задание: Модуль 08

## Цель домашнего задания

Цель — отработать стабильные локаторы Playwright, assertions и auto-waiting в отдельном репозитории домашних заданий.

Вам нужно:

- до начала работы обновить ветку модуля 08 из личной ветки `master`;
- создать тесты в `tests/module-08-locators-assertions/`;
- заполнить `homework/module-08-locators-assertions/result.md`;
- запустить тесты из терминала;
- при необходимости открыть HTML report;
- открыть PR в личную ветку `master`.

## Задание 1. Подготовить ветку модуля 08

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-08-locators-assertions
git merge origin/student/{student-name-slug}/master
git push
```

## Задание 2. Подготовить проект локально

Установите зависимости:

```bash
npm install
```

Если браузеры отсутствуют, установите их:

```bash
npx playwright install
```

## Задание 3. Создать файлы для тестов

Создайте каталог:

```text
tests/module-08-locators-assertions/
```

Добавьте файлы:

```text
tests/module-08-locators-assertions/01-role-locators.spec.ts
tests/module-08-locators-assertions/02-text-and-form-locators.spec.ts
tests/module-08-locators-assertions/03-todo-assertions.spec.ts
tests/module-08-locators-assertions/04-auto-waiting.spec.ts
homework/module-08-locators-assertions/result.md
```

## Задание 4. Локаторы по роли

В `01-role-locators.spec.ts` создайте тесты для `https://playwright.dev/`.

Используйте:

- `getByRole('link', ...)`;
- `getByRole('heading', ...)`;
- хотя бы одну проверку `toBeVisible`;
- одну проверку перехода через `toHaveURL`.

Например, откройте главную страницу, нажмите `Get started` и проверьте заголовок открывшейся страницы.

## Задание 5. Локаторы по тексту и полям формы

В `02-text-and-form-locators.spec.ts` используйте простую демонстрационную страницу:

```text
https://demo.playwright.dev/todomvc/
```

Примените `getByPlaceholder`, `getByText`, `fill`, `press` и `toBeVisible`. Добавьте одну задачу в список и проверьте, что она видна.

## Задание 6. Практика assertions

В `03-todo-assertions.spec.ts` создайте тест, который:

- открывает демонстрационную страницу TodoMVC;
- добавляет две задачи;
- проверяет видимость обеих задач;
- проверяет количество задач;
- по возможности отмечает одну задачу выполненной;
- проверяет ожидаемый видимый результат.

Используйте осмысленные assertions. Не заменяйте их выводом через `console.log`.

## Задание 7. Практика auto-waiting

В `04-auto-waiting.spec.ts` создайте тест, который выполняет действие и проверяет результат через assertion, но не использует `waitForTimeout`.

Добавьте комментарий о том, почему ожидание нужного результата лучше фиксированного интервала. Плохой вариант оставьте только как закомментированный пример:

```ts
// await page.waitForTimeout(5000);
```

В `result.md` объясните, почему фиксированные ожидания обычно вредны.

## Задание 8. Запустить тесты

```bash
npm run test
```

Запустите тесты в headed mode:

```bash
npm run test:headed
```

При необходимости откройте отчет:

```bash
npm run report
```

## Задание 9. Заполнить `result.md`

В `homework/module-08-locators-assertions/result.md` укажите:

- название текущей ветки;
- созданные файлы;
- выполненные команды;
- результат `npm run test`;
- примеры использованных локаторов;
- краткое объяснение `getByRole`;
- краткое объяснение `getByText`;
- краткое объяснение `getByPlaceholder`;
- определение assertion;
- объяснение auto-waiting;
- причину, по которой `waitForTimeout` обычно следует избегать;
- самую сложную проблему с locator;
- оставшиеся вопросы или проблемы.

## Задание 10. Commit, push и PR

Перед commit еще раз убедитесь, что тесты проходят:

```bash
npm run test
```

Затем сохраните и отправьте изменения:

```bash
git status
git add .
git commit -m "Complete module 08 locators assertions homework"
git push
```

Создайте PR со следующими ветками:

```text
base: student/{student-name-slug}/master
compare: student/{student-name-slug}/module-08-locators-assertions
```

## Ожидаемый результат

- [ ] Ветка модуля 08 обновлена из личной ветки `master` до начала работы.
- [ ] Тесты созданы в `tests/module-08-locators-assertions/`.
- [ ] Файл `result.md` заполнен.
- [ ] `npm run test` завершается успешно.
- [ ] Сгенерированные отчеты не добавлены в Git.
- [ ] В итоговых тестах нет фиксированных ожиданий.
- [ ] Изменения сохранены в commit и отправлены в удаленный репозиторий.
- [ ] PR открыт в личную ветку `master`.
- [ ] `node_modules` не добавлен в Git.
- [ ] В коде и документации нет настоящих имен учащихся.

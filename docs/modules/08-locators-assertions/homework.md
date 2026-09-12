# Домашнее задание: Модуль 08

## Цель домашнего задания

Работа выполняется в [репозитории домашних заданий](https://github.com/EugeneChirkun/Pw-Ts-Qa-Hw), а не в репозитории документации курса.

Цель — отработать стабильные локаторы Playwright, assertions и auto-waiting в отдельном репозитории домашних заданий.

Вам нужно:

- до начала работы обновить ветку модуля 08 из личной ветки `master`;
- создать тесты в `tests/module-08-locators-assertions/`;
- заполнить `homework/module-08-locators-assertions/result.md`;
- запустить тесты из терминала;
- при необходимости открыть HTML report;
- открыть PR в личную ветку `master`.

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

git switch --track origin/student/{student-name-slug}/module-08-locators-assertions
git merge origin/student/{student-name-slug}/master
```

Если локальные ветки уже существуют, используйте более короткие команды:

```bash
git fetch origin --prune

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-08-locators-assertions
git merge origin/student/{student-name-slug}/master
```

Если ветка модуля не найдена, не создавайте ветку со случайным названием. Обратитесь к преподавателю.

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

Не открывайте PR в `master` репозитория. Домашние задания объединяются в личную основную ветку студента `student/{student-name-slug}/master`.

## Ожидаемый результат

- Практическая работа выполнена в репозитории домашних заданий.
- Использована подготовленная ветка `student/{student-name-slug}/module-08-locators-assertions`.
- Перед началом работы ветка модуля обновлена из личной ветки `student/{student-name-slug}/master`.
- Требуемые файлы модуля созданы или обновлены, а `result.md` заполнен, если он предусмотрен заданием.
- `npm run typecheck` и тесты запущены, если они предусмотрены заданием.
- PR открыт в `student/{student-name-slug}/master`.
- `node_modules`, отчёты, состояние авторизации и реальные учётные данные не добавлены в Git.
- [ ] Ветка модуля 08 обновлена из личной ветки `master` до начала работы.
- [ ] Тесты созданы в `tests/module-08-locators-assertions/`.
- [ ] Файл `result.md` заполнен.
- [ ] `npm run test` завершается успешно.
- [ ] `npm run typecheck` завершается без ошибок.
- [ ] Сгенерированные отчеты не добавлены в Git.
- [ ] В итоговых тестах нет фиксированных ожиданий.
- [ ] Изменения сохранены в commit и отправлены в удаленный репозиторий.
- [ ] PR открыт в личную ветку `master`.
- [ ] `node_modules` не добавлен в Git.
- [ ] В коде и документации нет настоящих имен учащихся.

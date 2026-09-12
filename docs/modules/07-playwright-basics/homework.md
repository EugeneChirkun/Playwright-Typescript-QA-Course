# Домашнее задание: Модуль 07

## Цель домашнего задания

Работа выполняется в [репозитории домашних заданий](https://github.com/EugeneChirkun/Pw-Ts-Qa-Hw), а не в репозитории документации курса.

Цель — написать первые простые UI-тесты Playwright и запустить их локально в отдельном репозитории домашних заданий. Ссылка на него приведена на странице [ресурсов](resources.md).

Вам нужно:

- перед началом обновить ветку модуля 07 из личной ветки `master`;
- создать файлы с тестами в `tests/module-07-playwright-basics/`;
- заполнить `homework/module-07-playwright-basics/result.md`;
- запустить тесты из терминала;
- открыть HTML report;
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

git switch --track origin/student/{student-name-slug}/module-07-playwright-basics
git merge origin/student/{student-name-slug}/master
```

Если локальные ветки уже существуют, используйте более короткие команды:

```bash
git fetch origin --prune

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-07-playwright-basics
git merge origin/student/{student-name-slug}/master
```

Если ветка модуля не найдена, не создавайте ветку со случайным названием. Обратитесь к преподавателю.

## Задание 2. Подготовить проект локально

Установите зависимости:

```bash
npm install
```

Если браузеры Playwright ещё не установлены локально, установите их:

```bash
npx playwright install
```

## Задание 3. Создать файлы для тестов

Создайте каталог:

```text
tests/module-07-playwright-basics/
```

Подготовьте файлы:

```text
tests/module-07-playwright-basics/01-homepage.spec.ts
tests/module-07-playwright-basics/02-navigation.spec.ts
tests/module-07-playwright-basics/03-actions.spec.ts
tests/module-07-playwright-basics/04-intentional-failure.spec.ts
homework/module-07-playwright-basics/result.md
```

## Задание 4. Проверить главную страницу

В `01-homepage.spec.ts` создайте тест, который:

- открывает `https://playwright.dev/`;
- проверяет заголовок страницы;
- проверяет видимость главного заголовка.

Используйте `page.goto`, `expect(page).toHaveTitle` и `expect(locator).toBeVisible`.

## Задание 5. Проверить переход

В `02-navigation.spec.ts` создайте тест, который:

- открывает `https://playwright.dev/`;
- нажимает `Get started`;
- проверяет, что изменился URL или заголовок страницы.

Используйте простой locator. Подробная стратегия выбора locators будет рассмотрена в модуле 08.

## Задание 6. Проверить простые действия

В `03-actions.spec.ts` создайте тест для демонстрационной страницы:

```text
https://demo.playwright.dev/todomvc/
```

Тест должен:

- открыть страницу;
- заполнить поле новой задачи;
- нажать `Enter`;
- проверить видимость созданной задачи.

## Задание 7. Изучить намеренное падение

В `04-intentional-failure.spec.ts` создайте один тест, который намеренно падает. Например, проверьте неправильный заголовок страницы. Запустите тест, изучите сообщение об ошибке и откройте отчёт.

После этого исправьте тест перед итоговым PR или оставьте падающий пример закомментированным. В итоговом PR все активные тесты должны проходить.

## Задание 8. Запустить тесты

Запустите тесты из терминала:

```bash
npm run test
```

Запустите тесты в headed mode:

```bash
npm run test:headed
```

Откройте UI mode:

```bash
npm run test:ui
```

Откройте HTML report:

```bash
npm run report
```

## Задание 9. Заполнить `result.md`

В `homework/module-07-playwright-basics/result.md` укажите:

- название текущей ветки;
- созданные файлы;
- использованные команды;
- прошёл ли `npm run test`;
- пробовали ли вы headed mode;
- пробовали ли вы UI mode;
- открывали ли вы HTML report;
- краткое объяснение назначения `test`;
- краткое объяснение назначения `page`;
- краткое объяснение назначения `expect`;
- краткое объяснение необходимости `await`;
- ошибку, которую вы увидели при намеренном падении;
- оставшиеся вопросы или проблемы.

## Задание 10. Создать commit, отправить изменения и открыть PR

Перед commit убедитесь, что все активные тесты проходят:

```bash
npm run test
```

Затем выполните:

```bash
git status
git add .
git commit -m "Complete module 07 Playwright basics homework"
git push
```

Откройте PR со следующими ветками:

```text
base: student/{student-name-slug}/master
compare: student/{student-name-slug}/module-07-playwright-basics
```

Не открывайте PR в `master` репозитория. Домашние задания объединяются в личную основную ветку студента `student/{student-name-slug}/master`.

## Ожидаемый результат

- Практическая работа выполнена в репозитории домашних заданий.
- Использована подготовленная ветка `student/{student-name-slug}/module-07-playwright-basics`.
- Перед началом работы ветка модуля обновлена из личной ветки `student/{student-name-slug}/master`.
- Требуемые файлы модуля созданы или обновлены, а `result.md` заполнен, если он предусмотрен заданием.
- `npm run typecheck` и тесты запущены, если они предусмотрены заданием.
- PR открыт в `student/{student-name-slug}/master`.
- `node_modules`, отчёты, состояние авторизации и реальные учётные данные не добавлены в Git.
- [ ] Ветка модуля 07 обновлена из личной ветки `master` перед началом работы.
- [ ] Файлы с тестами созданы в `tests/module-07-playwright-basics/`.
- [ ] Файл `result.md` заполнен.
- [ ] `npm run test` завершается успешно.
- [ ] `npm run typecheck` завершается без ошибок.
- [ ] `node_modules`, `playwright-report` и `test-results` не добавлены в Git.
- [ ] Изменения сохранены в commit и отправлены в удалённый репозиторий.
- [ ] PR открыт в личную ветку `master`.
- [ ] В коде и документации нет настоящих имён учащихся.

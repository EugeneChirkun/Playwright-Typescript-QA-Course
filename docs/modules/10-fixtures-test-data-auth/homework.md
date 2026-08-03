# Домашнее задание: Модуль 10

## Цель домашнего задания

Работа выполняется в [репозитории домашних заданий](https://github.com/EugeneChirkun/Pw-Ts-Qa-Hw), а не в репозитории документации курса.

Цель — закрепить работу с fixtures, организацией test data и базовым auth setup в Playwright.

Вам нужно обновить ветку модуля из личной ветки `master`, создать test data и custom fixture, подключить Page Object через fixture, подготовить пример auth setup, заполнить `homework/module-10-fixtures-test-data-auth/result.md`, выполнить доступные проверки и открыть PR в личную ветку `master`.

Работа выполняется в репозитории домашних заданий `Pw-Ts-Qa-Hw`, а не в репозитории материалов курса.

## Задание 1. Подготовить ветку модуля 10

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-10-fixtures-test-data-auth
git merge origin/student/{student-name-slug}/master
git push
```

Убедитесь, что merge завершился без неразрешенных конфликтов.

## Задание 2. Создать файлы

Создайте или обновите следующую структуру в репозитории домашних заданий:

```text
src/test-data/users.ts
tests/fixtures/pages.fixture.ts
tests/auth.setup.ts
tests/module-10/fixtures-auth.spec.ts
homework/module-10-fixtures-test-data-auth/result.md
```

Если каких-либо каталогов нет, создайте их.

## Задание 3. Подготовить test data

В `src/test-data/users.ts` создайте и экспортируйте:

- `type UserRole = 'admin' | 'manager' | 'viewer'`;
- `interface TestUser`;
- `adminUser`;
- `managerUser`;
- `viewerUser`;
- массив `users`.

Используйте только демонстрационные учетные данные. Не указывайте реальные пароли и учетные записи.

## Задание 4. Создать custom fixture для Page Objects

В `tests/fixtures/pages.fixture.ts` создайте через `test.extend` custom fixture, которая предоставляет `loginPage`. Если в проекте уже есть `DashboardPage`, добавьте также `dashboardPage`; иначе оставьте только `loginPage`.

Экспортируйте custom `test` и `expect`.

## Задание 5. Написать тест с fixture

В `tests/module-10/fixtures-auth.spec.ts` создайте хотя бы один тест, который:

- импортирует custom `test` и `expect`;
- получает `loginPage` из fixture;
- открывает страницу входа;
- выполняет одно простое действие или проверку.

Не создавайте `LoginPage` вручную, если его уже предоставляет fixture.

## Задание 6. Подготовить пример auth setup

В `tests/auth.setup.ts` создайте setup-тест, который:

- импортирует `LoginPage` и `adminUser`;
- открывает страницу входа;
- входит как администратор;
- сохраняет состояние в файл:

```text
playwright/.auth/admin.json
```

Если учебное приложение пока не поддерживает вход, создайте структуру и добавьте комментарии о предполагаемых шагах. Не используйте реальные учетные данные.

## Задание 7. Обновить `.gitignore`

Убедитесь, что `.gitignore` содержит:

```text
playwright/.auth/
```

Не добавляйте созданные auth-файлы в Git.

## Задание 8. При необходимости описать конфигурацию

Если в проекте уже есть `playwright.config.ts`, добавьте или опишите простой setup project только при условии, что это не нарушит существующие тесты. Не усложняйте конфигурацию.

Если демонстрационный вход недоступен и изменение рискованно, не добавляйте setup project принудительно. Опишите предполагаемую конфигурацию в `result.md`.

## Задание 9. Запустить проверки

```bash
npm run typecheck
```

Если тесты можно выполнить:

```bash
npm run test
```

Если тесты нельзя запустить из-за отсутствия страницы входа или учетных данных, объясните это в `result.md`. Проверка TypeScript все равно должна проходить.

## Задание 10. Заполнить `result.md`

В `homework/module-10-fixtures-test-data-auth/result.md` укажите:

- имя текущей ветки;
- созданные и обновленные файлы;
- выполненные команды;
- результат `npm run typecheck`;
- запускались ли тесты и каков результат;
- краткое объяснение fixture и custom fixture;
- краткое объяснение `test.extend`;
- краткое объяснение организации test data;
- краткое объяснение `storageState`;
- почему auth-файлы нельзя добавлять в Git;
- возникшие вопросы или проблемы.

## Задание 11. Создать commit, отправить изменения и открыть PR

```bash
git status
git add .
git commit -m "Complete module 10 fixtures test data auth homework"
git push
```

Откройте PR со следующими ветками:

```text
base: student/{student-name-slug}/master
compare: student/{student-name-slug}/module-10-fixtures-test-data-auth
```

## Ожидаемый результат

- Ветка модуля 10 обновлена из личной ветки `master` до начала работы.
- Создан файл test data.
- Создан custom fixture.
- Хотя бы один тест использует custom fixture.
- Подготовлен пример auth setup.
- `.gitignore` защищает `playwright/.auth/`.
- Заполнен `result.md`.
- `npm run typecheck` проходит успешно.
- Тесты запущены, если учебное приложение это позволяет.
- Изменения сохранены в commit и отправлены на GitHub.
- Открыт PR в личную ветку `master`.
- В Git не добавлены `node_modules`, созданные отчеты, auth-файлы и реальные учетные данные.
- В коде и документации не используются реальные имена студентов.

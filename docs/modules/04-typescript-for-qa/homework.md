# Домашнее задание: Модуль 04

## Цель домашнего задания

Цель домашнего задания — потренировать TypeScript в QA-сценариях:

* типизированные пользователи;
* типизированные тест-кейсы;
* union types;
* helper functions;
* `export` и `import`;
* typecheck.

Студенту нужно:

* обновить ветку модуля 04 из личной ветки `master` перед началом работы;
* создать TypeScript-файлы в `src/training/module-04-typescript-for-qa/`;
* заполнить `homework/module-04-typescript-for-qa/result.md`;
* запустить `npm run typecheck`;
* открыть pull request в личную ветку `master`.

## Задание 1. Подготовить ветку модуля 04

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-04-typescript-for-qa
git merge origin/student/{student-name-slug}/master
git push
```

## Задание 2. Создать файлы для упражнений

Создайте папку:

```text
src/training/module-04-typescript-for-qa/
```

Создайте файлы:

```text
src/training/module-04-typescript-for-qa/types.ts
src/training/module-04-typescript-for-qa/users.ts
src/training/module-04-typescript-for-qa/test-cases.ts
src/training/module-04-typescript-for-qa/helpers.ts
src/training/module-04-typescript-for-qa/index.ts
homework/module-04-typescript-for-qa/result.md
```

## Задание 3. Создать типы для пользователей и тест-кейсов

В `types.ts` создайте:

* `type UserRole = 'admin' | 'manager' | 'viewer'`;
* `type TestPriority = 'low' | 'medium' | 'high'`;
* `type TestStatus = 'draft' | 'ready' | 'blocked'`;
* `interface LoginCredentials`;
* `interface TestUser`;
* `interface TestCase`.

Требования:

* `TestUser` должен содержать email, password, role и isActive.
* `TestCase` должен содержать id, title, priority, status и необязательное поле description.
* Все типы и интерфейсы должны быть экспортированы.

## Задание 4. Создать тестовых пользователей

В `users.ts` импортируйте нужные типы.

Создайте и экспортируйте:

* `adminUser`;
* `managerUser`;
* `viewerUser`;
* `inactiveUser`;
* массив `users`.

Используйте корректные типы.

## Задание 5. Создать тест-кейсы

В `test-cases.ts` импортируйте нужные типы.

Создайте и экспортируйте:

* минимум три тест-кейса;
* один тест-кейс с высоким приоритетом;
* один заблокированный тест-кейс;
* один тест-кейс с необязательным описанием;
* массив `testCases`.

## Задание 6. Создать helper functions

В `helpers.ts` создайте и экспортируйте функции:

* `createUserEmail(prefix: string): string`;
* `isActiveUser(user: TestUser): boolean`;
* `isHighPriority(testCase: TestCase): boolean`;
* `getReadyTestCases(testCases: TestCase[]): TestCase[]`;
* `formatBugTitle(moduleName: string, issue: string): string`;
* `createTestUser(role: UserRole): TestUser`.

Используйте imports из `types.ts`.

## Задание 7. Собрать пример использования в `index.ts`

В `index.ts`:

* импортируйте пользователей;
* импортируйте тест-кейсы;
* импортируйте helper functions;
* вызовите несколько helper functions;
* сохраните результаты в переменные;
* выведите несколько результатов через `console.log`.

Не усложняйте пример: здесь важна понятная структура файлов и корректные типы.

## Задание 8. Запустить typecheck

```bash
npm run typecheck
```

Если typecheck не проверяет `src/training/**/*.ts`, обновите `tsconfig.json`, чтобы файлы в `src/**/*.ts` попадали в проверку. Не исключайте файлы модуля 04 из typecheck.

Исправьте все TypeScript-ошибки до открытия pull request.

## Задание 9. Заполнить `result.md`

В `homework/module-04-typescript-for-qa/result.md` добавьте:

* название текущей ветки;
* список созданных файлов;
* использованные команды;
* прошел ли `npm run typecheck`;
* краткое объяснение типизированных test data;
* краткое объяснение union type;
* краткое объяснение optional property;
* краткое объяснение helper function;
* краткое объяснение `export` и `import`;
* вопросы или проблемы, если они появились.

## Задание 10. Сделать commit, push и pull request

```bash
git status
git add .
git commit -m "Complete module 04 TypeScript for QA homework"
git push
```

Pull request:

```text
base: student/{student-name-slug}/master
compare: student/{student-name-slug}/module-04-typescript-for-qa
```

Ожидаемый результат:

* ветка модуля 04 обновлена из личной ветки `master` до начала работы;
* TypeScript-файлы созданы в `src/training/module-04-typescript-for-qa/`;
* `result.md` заполнен;
* `npm run typecheck` проходит успешно;
* изменения закоммичены и отправлены в удаленный репозиторий;
* pull request открыт в личную ветку `master`;
* `node_modules` не попал в commit;
* сгенерированные отчеты не попали в commit;
* реальные имена студентов не используются в коде и документации.

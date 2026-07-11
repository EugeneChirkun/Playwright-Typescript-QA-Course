# Домашнее задание: Модуль 03

## Цель домашнего задания

Цель домашнего задания — пошагово потренировать основы JavaScript и TypeScript в отдельном репозитории для домашних заданий: <https://github.com/EugeneChirkun/Pw-Ts-Qa-Hw>.

Вам нужно:

- перед началом обновить ветку модуля 03 из личной ветки `master`;
- создать небольшие TypeScript-файлы с упражнениями;
- написать простой код;
- запустить `npm run typecheck`;
- открыть pull request в личную ветку `master`.

`{student-name-slug}` означает нейтральный slug студента в названии ветки.

## Задание 1. Подготовить ветку модуля 03

Выполните команды перед созданием или редактированием файлов:

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-03-typescript-basics
git merge origin/student/{student-name-slug}/master
git push
```

Так ветка модуля 03 начнёт работу с актуального состояния личной ветки `master`.

## Задание 2. Создать файлы для упражнений

Создайте папку:

```text
homework/module-03-typescript-basics/exercises/
```

Создайте файлы:

```text
homework/module-03-typescript-basics/exercises/01-values-variables.ts
homework/module-03-typescript-basics/exercises/02-arrays-loops.ts
homework/module-03-typescript-basics/exercises/03-objects.ts
homework/module-03-typescript-basics/exercises/04-conditions.ts
homework/module-03-typescript-basics/exercises/05-functions.ts
homework/module-03-typescript-basics/exercises/06-types-interfaces.ts
homework/module-03-typescript-basics/result.md
```

## Задание 3. Значения и переменные

В файле `01-values-variables.ts` создайте переменные:

- `baseUrl`;
- `userEmail`;
- `userPassword`;
- `maxRetries`;
- `timeoutMs`;
- `isHeadless`;
- `isUserLoggedIn`.

Используйте `const`, где значение не меняется, и `let`, где значение должно измениться.

Дополнительно:

- создайте `currentRetry`;
- увеличьте `currentRetry` на `1`.

Добавьте неправильный пример в комментарий и объясните в `result.md`, почему он неправильный:

```ts
// const retryCount: number = 'three';
```

## Задание 4. Массивы и циклы

В файле `02-arrays-loops.ts` создайте:

- массив браузеров;
- массив ролей пользователей;
- массив status codes.

Затем:

- выведите каждый браузер через `for...of`;
- выведите каждую роль через `for...of`;
- получите первый status code;
- получите длину массива.

## Задание 5. Объекты

В файле `03-objects.ts` создайте объекты:

- `testUser`;
- `adminUser`;
- `testCase`.

У каждого объекта должно быть несколько полей. Например, для пользователя можно использовать email, password, role и флаг активности. Для тест-кейса можно использовать id, title и priority.

Затем:

- прочитайте минимум два поля из каждого объекта;
- выведите эти значения через `console.log`.

## Задание 6. Условия

В файле `04-conditions.ts` создайте:

- переменную со status code;
- проверку успешного status code через `if`;
- проверку залогиненного пользователя через `if/else`;
- простую проверку теста с высоким приоритетом.

Держите код простым и понятным.

## Задание 7. Функции

В файле `05-functions.ts` создайте функции:

- `createUserEmail(prefix: string): string`;
- `isSuccessStatus(statusCode: number): boolean`;
- `getTestTitle(featureName: string, scenarioName: string): string`;
- `getLoginMessage(isLoggedIn: boolean): string`.

Вызовите каждую функцию и сохраните результат в отдельную переменную.

## Задание 8. Первые `type` и `interface`

В файле `06-types-interfaces.ts` создайте:

- `type UserRole = 'admin' | 'manager' | 'viewer'`;
- `type TestPriority = 'low' | 'medium' | 'high'`;
- `interface TestUser`;
- `interface TestCase`.

Затем создайте:

- один объект `TestUser`;
- один объект `TestCase`;
- одно optional property, например `description?: string`.

Это первое знакомство с `type` и `interface`, поэтому не усложняйте решение.

## Задание 9. Запустить typecheck

Запустите проверку типов:

```bash
npm run typecheck
```

Если появились TypeScript errors, исправьте их до открытия PR. Если ошибка связана с намеренно неправильным примером, убедитесь, что этот пример закомментирован.

## Задание 10. Заполнить result.md

В файле `homework/module-03-typescript-basics/result.md` укажите:

- текущее название ветки;
- созданные файлы;
- использованные команды;
- прошёл ли `npm run typecheck`;
- краткое объяснение `const` и `let`;
- краткое объяснение `if`;
- краткое объяснение `for...of`;
- краткое объяснение функции и `return`;
- краткое объяснение `type` и `interface`;
- вопросы или проблемы, если они есть.

## Задание 11. Commit, push и PR

Выполните команды:

```bash
git status
git add .
git commit -m "Complete module 03 JavaScript and TypeScript basics homework"
git push
```

Откройте PR:

```text
base: student/{student-name-slug}/master
compare: student/{student-name-slug}/module-03-typescript-basics
```

Ожидаемый результат:

- ветка модуля 03 обновлена из личной ветки `master` до начала работы;
- файлы с упражнениями созданы;
- простые основы JavaScript и TypeScript отработаны;
- `result.md` заполнен;
- `npm run typecheck` проходит;
- изменения закоммичены и отправлены в удалённый репозиторий;
- PR открыт в личную ветку `master`;
- `node_modules` не попал в commit;
- сгенерированные отчёты не попали в commit.

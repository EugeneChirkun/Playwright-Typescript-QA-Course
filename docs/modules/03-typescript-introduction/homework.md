# Домашнее задание: Модуль 03

## Цель домашнего задания

Цель домашнего задания — отработать основы TypeScript в домашнем репозитории: <https://github.com/EugeneChirkun/Pw-Ts-Qa-Hw>.

Вам нужно:

- работать в ветке `student/<student-name-slug>/module-03-typescript-basics`;
- перед началом обновить эту ветку из `student/<student-name-slug>/master`;
- создать TypeScript-файлы с упражнениями;
- запустить typecheck;
- открыть PR в `student/<student-name-slug>/master`.

## Задание 1. Подготовить ветку модуля 03

Выполните команды перед созданием или редактированием файлов для модуля 03:

```bash id="0tqo7t"
git fetch origin

git switch student/<student-name-slug>/master
git pull origin student/<student-name-slug>/master

git switch student/<student-name-slug>/module-03-typescript-basics
git merge origin/student/<student-name-slug>/master
git push
```

Так вы начнёте работу с актуального состояния личной ветки `master`.

## Задание 2. Создать папку для упражнений

Создайте папку:

```text id="ubgqpl"
homework/module-03-typescript-basics/exercises/
```

Создайте файлы:

```text id="v1lhng"
homework/module-03-typescript-basics/exercises/01-variables.ts
homework/module-03-typescript-basics/exercises/02-arrays.ts
homework/module-03-typescript-basics/exercises/03-objects.ts
homework/module-03-typescript-basics/exercises/04-functions.ts
homework/module-03-typescript-basics/exercises/05-types-interfaces.ts
homework/module-03-typescript-basics/result.md
```

## Задание 3. Переменные и базовые типы

В файле `01-variables.ts` создайте переменные:

- `baseUrl`;
- `userEmail`;
- `userPassword`;
- `maxRetries`;
- `isHeadless`;
- `timeoutMs`.

Используйте подходящие TypeScript-типы: `string`, `number` и `boolean`.

Также добавьте одну намеренно неправильную строку, закомментируйте её и объясните в `result.md`, почему она неправильная.

```ts id="9i1i2x"
// const retryCount: number = 'three';
```

## Задание 4. Массивы

В файле `02-arrays.ts` создайте:

- массив названий браузеров;
- массив кодов статусов;
- массив приоритетов тестов.

Добавьте:

- проход по названиям браузеров через `for...of`;
- функцию, которая проверяет, является ли код статуса успешным.

## Задание 5. Объекты

В файле `03-objects.ts` создайте объекты:

- `testUser`;
- `adminUser`;
- `testCase`.

Поля могут включать:

- email;
- password;
- role;
- isActive;
- название теста;
- priority;
- expected result.

## Задание 6. Функции

В файле `04-functions.ts` создайте функции:

- `createUserEmail(prefix: string): string`;
- `isSuccessStatus(statusCode: number): boolean`;
- `getTestTitle(featureName: string, scenarioName: string): string`;
- `formatBugTitle(moduleName: string, issue: string): string`.

Функции должны быть простыми, читаемыми и предсказуемыми.

## Задание 7. type и interface

В файле `05-types-interfaces.ts` создайте:

- `type UserRole = 'admin' | 'manager' | 'viewer'`;
- `type TestPriority = 'low' | 'medium' | 'high'`;
- `interface TestUser`;
- `interface TestCase`.

Создайте минимум:

- один объект `TestUser`;
- один объект `TestCase`;
- одно optional property, например `description?: string`.

## Задание 8. Запустить typecheck

Запустите проверку типов:

```bash id="rha1ad"
npm run typecheck
```

Если команда недоступна, проверьте `package.json`. Если появились ошибки, исправьте TypeScript-ошибки до открытия PR.

## Задание 9. Заполнить result.md

В файле `homework/module-03-typescript-basics/result.md` укажите:

- текущее название ветки;
- созданные файлы;
- использованные команды;
- прошёл ли `npm run typecheck`;
- краткое объяснение `type` и `interface`;
- краткое объяснение optional property;
- краткое объяснение union type;
- вопросы или проблемы, если они есть.

## Задание 10. Commit, push и PR

Выполните команды:

```bash id="ktlxvw"
git status
git add .
git commit -m "Complete module 03 TypeScript basics homework"
git push
```

Откройте PR:

```text id="4ufbjg"
base: student/<student-name-slug>/master
compare: student/<student-name-slug>/module-03-typescript-basics
```

Ожидаемый результат:

- ветка модуля 03 обновлена из личной ветки `master` до начала работы;
- TypeScript-файлы с упражнениями созданы;
- `result.md` заполнен;
- `npm run typecheck` проходит;
- изменения закоммичены и отправлены в удалённый репозиторий;
- PR открыт в личную ветку `master`;
- `node_modules` не попал в commit;
- сгенерированные отчёты не попали в commit.

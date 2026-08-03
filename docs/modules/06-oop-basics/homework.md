# Домашнее задание: Модуль 06

## Цель домашнего задания

Работа выполняется в [репозитории домашних заданий](https://github.com/EugeneChirkun/Pw-Ts-Qa-Hw), а не в репозитории документации курса.

Цель — закрепить основы ООП в TypeScript на примерах из QA. Перед началом обновите ветку модуля 06 из личной ветки `master`, создайте TypeScript-файлы в `src/training/module-06-oop-basics/`, заполните `homework/module-06-oop-basics/result.md`, запустите `npm run typecheck` и откройте PR в личную ветку `master`.

Работа выполняется в отдельном репозитории домашних заданий `Pw-Ts-Qa-Hw`, а не в репозитории материалов курса.

## Задание 1. Подготовить ветку модуля 06

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-06-oop-basics
git merge origin/student/{student-name-slug}/master
git push
```

## Задание 2. Создать файлы для упражнений

Создайте каталог:

```text
src/training/module-06-oop-basics/
```

Создайте файлы:

```text
src/training/module-06-oop-basics/01-classes-objects.ts
src/training/module-06-oop-basics/02-constructors-methods.ts
src/training/module-06-oop-basics/03-encapsulation.ts
src/training/module-06-oop-basics/04-composition.ts
src/training/module-06-oop-basics/05-inheritance-overview.ts
src/training/module-06-oop-basics/index.ts
homework/module-06-oop-basics/result.md
```

## Задание 3. Classes и objects

В `01-classes-objects.ts` создайте class `TestUser` с properties `email`, `role`, `isActive` и constructor. Создайте два объекта: `adminUser` и `viewerUser`. Выведите несколько их properties через `console.log`.

## Задание 4. Constructors и methods

В `02-constructors-methods.ts` создайте class `TestCase`. Добавьте:

- `id`, `title` и `priority`;
- constructor;
- method `getFullTitle(): string`;
- method `isHighPriority(): boolean`.

Создайте не менее двух тест-кейсов и вызовите оба methods.

## Задание 5. Инкапсуляция

В `03-encapsulation.ts` создайте class `LoginCredentials`. Добавьте:

- public readonly property `email`;
- private property `password`;
- method `checkPassword(password: string): boolean`;
- method `getMaskedPassword(): string`.

Не обращайтесь к private property `password` извне класса. Добавьте закомментированный неверный пример и объясните в `result.md`, почему он неверен:

```ts
// console.log(credentials.password);
```

## Задание 6. Composition

В `04-composition.ts` создайте classes `TestUser` и `TestRun`. `TestRun` должен получать `TestUser` через constructor, хранить `testName` и `user`, а также иметь method `getDescription(): string`. Создайте пользователя, передайте его в новый тестовый запуск и вызовите `getDescription`.

## Задание 7. Обзор inheritance

В `05-inheritance-overview.ts` создайте:

- class `BaseUser` с `email` и method `getEmail()`;
- class `AdminUser extends BaseUser` с method `canManageUsers()`.

Добавьте короткий комментарий: inheritance показано только обзорно; в большинстве примеров курса предпочтительны простые classes и composition.

## Задание 8. Подготовить index.ts

В `index.ts` импортируйте или вызовите несколько примеров из файлов упражнений. Не усложняйте решение и убедитесь, что typecheck проходит. Если imports и exports пока вызывают трудности, можно оставить примеры независимыми, но `npm run typecheck` должен завершаться без ошибок.

## Задание 9. Запустить typecheck

```bash
npm run typecheck
```

Исправьте все ошибки TypeScript до создания PR.

## Задание 10. Заполнить result.md

В `homework/module-06-oop-basics/result.md` укажите:

- текущую ветку;
- созданные файлы;
- использованные команды;
- результат `npm run typecheck`;
- краткие объяснения class, object и constructor;
- краткие объяснения properties, methods и `this`;
- назначение `public`, `private` и `readonly`;
- краткие объяснения composition и inheritance;
- возникшие вопросы или проблемы.

## Задание 11. Создать commit, отправить изменения и открыть PR

```bash
git status
git add .
git commit -m "Complete module 06 OOP basics homework"
git push
```

Параметры PR:

```text
base: student/{student-name-slug}/master
compare: student/{student-name-slug}/module-06-oop-basics
```

## Ожидаемый результат

- ветка модуля 06 обновлена из личной ветки `master` до начала работы;
- TypeScript-файлы созданы в `src/training/module-06-oop-basics/`;
- `result.md` заполнен;
- `npm run typecheck` проходит;
- изменения сохранены в commit и отправлены на GitHub;
- PR открыт в личную ветку `master`;
- `node_modules` и сгенерированные отчёты не добавлены в Git;
- в коде и документации нет настоящих имён студентов.

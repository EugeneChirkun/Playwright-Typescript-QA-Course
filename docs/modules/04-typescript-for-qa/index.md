# Модуль 04: TypeScript для QA

## Цель модуля

Цель модуля — понять, как TypeScript помогает организовывать код для автоматизации QA до того, как мы начнем глубоко использовать Playwright.

После этого модуля студент сможет:

* описывать test data через `type` и `interface`;
* использовать union types для ролей, статусов и приоритетов;
* создавать типизированные объекты и массивы объектов;
* писать простые helper functions;
* разделять код на файлы с помощью `export` и `import`;
* подготавливать переиспользуемые данные для будущих Playwright-тестов.

## Что уже нужно знать

Перед модулем важно понимать базовые темы из модуля 03:

* `const` и `let`;
* базовые типы;
* массивы;
* объекты;
* функции;
* `if`;
* `for...of`;
* базовые `type` и `interface`.

## Почему TypeScript важен именно для QA

В автотестах часто повторяются одни и те же данные: пользователи, роли, статусы, приоритеты, формы и ожидаемые сообщения. Если хранить такие данные без типов, их легко случайно сломать: забыть обязательное поле, написать роль с опечаткой или передать не тот статус.

TypeScript помогает находить такие ошибки до запуска тестов. Типизированные helper functions проще переиспользовать, а понятные типизированные данные легче читать на code review в pull request.

## Темы модуля

* test data;
* `type` и `interface` на практическом уровне;
* union types для ролей, статусов и приоритетов;
* optional properties;
* массивы типизированных объектов;
* helper functions;
* типы возвращаемых значений функций;
* простые data factories;
* `export` и `import`;
* организация QA-кода в проекте;
* типичные ошибки.

## 1. Test data в автотестах

Test data — это данные, которые используются в тестах:

* пользователи;
* учетные данные;
* роли;
* ожидаемые сообщения;
* метаданные тест-кейсов;
* объекты, похожие на API-ответы;
* значения для форм.

```ts
const validUser = {
  email: 'qa.user@example.com',
  password: 'Password123!',
  role: 'viewer',
};
```

Если такие объекты повторяются в разных файлах без типов, в них легко допустить расхождения. Например, в одном месте роль может быть `viewer`, а в другом — `viewr`. TypeScript помогает ограничить допустимые значения и быстрее заметить ошибку.

## 2. Описание пользователя через `type`

```ts
type UserRole = 'admin' | 'manager' | 'viewer';

type TestUser = {
  email: string;
  password: string;
  role: UserRole;
  isActive: boolean;
};

const viewerUser: TestUser = {
  email: 'viewer@example.com',
  password: 'Password123!',
  role: 'viewer',
  isActive: true,
};
```

В этом примере:

* `UserRole` ограничивает допустимые значения роли;
* `TestUser` описывает структуру данных пользователя;
* TypeScript покажет ошибку, если пропустить обязательное поле или указать несуществующую роль.

## 3. Описание тест-кейса через `interface`

```ts
type TestPriority = 'low' | 'medium' | 'high';
type TestStatus = 'draft' | 'ready' | 'blocked';

interface TestCase {
  id: string;
  title: string;
  priority: TestPriority;
  status: TestStatus;
  description?: string;
}
```

`description?: string` означает, что поле необязательное. Его можно добавить, если нужно пояснение, но объект останется корректным и без него.

Union types помогают не использовать случайные значения. Например, вместо произвольного текста `'very important'` для приоритета можно выбрать только `'low'`, `'medium'` или `'high'`.

## 4. `type` или `interface`: что выбрать сейчас

На этом этапе важно не спорить о тонких различиях, а научиться применять типы в QA-коде.

* `type` и `interface` могут описывать форму объекта.
* В рамках курса достаточно выбрать один подход и использовать его последовательно.
* `interface` часто удобно использовать для структуры объектов.
* `type` удобно использовать для union types и простых псевдонимов.
* Продвинутые отличия разберем позже, когда появится практическая необходимость.

```ts
type UserRole = 'admin' | 'manager' | 'viewer';

interface LoginCredentials {
  email: string;
  password: string;
}
```

## 5. Массивы типизированных объектов

В тестах часто нужны несколько пользователей, ролей или тест-кейсов. Для этого удобно хранить массив типизированных объектов.

```ts
const users: TestUser[] = [
  {
    email: 'admin@example.com',
    password: 'Password123!',
    role: 'admin',
    isActive: true,
  },
  {
    email: 'viewer@example.com',
    password: 'Password123!',
    role: 'viewer',
    isActive: true,
  },
];
```

Такой массив можно перебрать через `for...of`:

```ts
for (const user of users) {
  console.log(`${user.email}: ${user.role}`);
}
```

## 6. Фильтрация test data

`filter` создает новый массив только с теми элементами, которые подходят под условие.

```ts
const activeUsers = users.filter((user) => user.isActive);
const adminUsers = users.filter((user) => user.role === 'admin');
```

Это полезно, когда тесту нужны не все данные, а только активные пользователи, администраторы или тест-кейсы с определенным статусом. На этом этапе достаточно понимать практическую идею: `filter` помогает выбрать подходящие элементы из массива.

## 7. Helper functions для QA

Helper function — это небольшая переиспользуемая функция, которая делает тестовый код чище и понятнее.

```ts
function createUserEmail(prefix: string): string {
  return `${prefix}@example.com`;
}

function isHighPriority(testCase: TestCase): boolean {
  return testCase.priority === 'high';
}

function formatBugTitle(moduleName: string, issue: string): string {
  return `[${moduleName}] ${issue}`;
}
```

Параметры функций должны иметь типы. Тип возвращаемого значения помогает сразу понять, что функция вернет: строку, boolean-значение, объект или массив.

## 8. Data factory: простое создание тестовых данных

Data factory — это функция, которая создает объект с тестовыми данными. Такой подход помогает не повторять почти одинаковые объекты вручную.

```ts
function createTestUser(role: UserRole): TestUser {
  return {
    email: `${role}.user@example.com`,
    password: 'Password123!',
    role,
    isActive: true,
  };
}

const admin = createTestUser('admin');
const viewer = createTestUser('viewer');
```

Такой шаблон пригодится позже, когда тестам понадобятся разные пользователи, формы и наборы данных.

## 9. `export` и `import`

Когда код растет, его нужно разделять на файлы: отдельно типы, отдельно данные, отдельно helper functions.

```ts
// src/training/module-04/types.ts
export type UserRole = 'admin' | 'manager' | 'viewer';

export interface TestUser {
  email: string;
  password: string;
  role: UserRole;
  isActive: boolean;
}
```

```ts
// src/training/module-04/users.ts
import { TestUser } from './types';

export const viewerUser: TestUser = {
  email: 'viewer@example.com',
  password: 'Password123!',
  role: 'viewer',
  isActive: true,
};
```

`export` делает тип, объект или функцию доступными для других файлов. `import` позволяет использовать их в другом файле. Это будет важно для Page Objects, helper functions и test data в следующих модулях.

## 10. Где хранить такой код в проекте

Для практики модуля 04 TypeScript-код нужно размещать в отдельном homework repository в папке:

```text
src/training/module-04-typescript-for-qa/
```

Письменный итог домашнего задания нужно разместить в файле:

```text
homework/module-04-typescript-for-qa/result.md
```

`src/` используется для переиспользуемого TypeScript-кода. `homework/` используется для письменных итогов, кратких объяснений и скриншотов. Позже Playwright-тесты будут размещаться в `tests/`.

## 11. Типичные ошибки

Типичные ошибки в этом модуле:

* использовать `string` там, где union type был бы безопаснее;
* создавать повторяющиеся нетипизированные объекты;
* забывать обязательные поля;
* добавлять случайные значения ролей;
* не экспортировать данные, которые должны переиспользоваться;
* импортировать файл по неправильному пути;
* слишком рано использовать `any`;
* усложнять типы без необходимости.

Избегайте `any` в этом модуле, если преподаватель явно не попросил использовать его для отдельного примера.

## Практика на занятии

1. Откройте homework repository в VS Code.
2. Проверьте текущую ветку.
3. Переключитесь на личную ветку `master` и получите обновления.
4. Переключитесь на ветку модуля 04.
5. Перед началом работы влейте личную ветку `master` в ветку модуля 04.
6. Создайте папку `src/training/module-04-typescript-for-qa/`.
7. Создайте типизированных пользователей.
8. Создайте типизированные тест-кейсы.
9. Создайте helper functions.
10. Разделите код на файлы с помощью `export` и `import`.
11. Запустите typecheck.

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-04-typescript-for-qa
git merge origin/student/{student-name-slug}/master
git push
```

```bash
npm run typecheck
```

## Вопросы для проверки понимания

* Что такое test data?
* Почему типизированные test data полезны в автотестах?
* Для чего используется `type`?
* Для чего используется `interface`?
* Когда полезен union type?
* Что такое optional property?
* Что такое массив типизированных объектов?
* Что делает `filter`?
* Что такое helper function?
* Почему параметры функции должны иметь типы?
* Что такое тип возвращаемого значения функции?
* Что такое data factory?
* Что делает `export`?
* Что делает `import`?
* Почему код стоит разделять на несколько файлов?
* Почему на этом этапе лучше избегать `any`?
* Где должен лежать TypeScript-код для модуля 04?
* Где должен лежать письменный итог домашнего задания?

## Краткий итог

В этом модуле студент научился описывать QA-данные с помощью TypeScript, создавать типизированных пользователей и тест-кейсы, писать простые helper functions и разделять код на файлы. Это подготовка к будущей работе с Playwright, Page Object Model и организацией test data.

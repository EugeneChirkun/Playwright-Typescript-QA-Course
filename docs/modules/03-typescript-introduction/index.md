# Модуль 03: TypeScript — основы

## Цель модуля

После этого модуля вы должны понимать базовый синтаксис TypeScript и уметь писать простой типизированный код, который позже будет использоваться в автоматизированных тестах.

В этом модуле вы разберёте:

- что такое TypeScript;
- зачем он нужен в Automation QA;
- чем TypeScript на базовом уровне отличается от JavaScript;
- как работать с базовыми типами;
- как писать простые функции;
- как описывать простые объекты;
- как читать ошибки TypeScript в VS Code.

## Почему TypeScript важен для Automation QA

Playwright-проекты часто пишут на TypeScript, потому что типы помогают раньше замечать ошибки в данных, параметрах функций и структурах объектов. Это особенно полезно для Automation QA: тестовые данные, вспомогательные функции и проверки становятся понятнее.

TypeScript также улучшает навигацию по коду и автодополнение в VS Code. Когда код хорошо типизирован, легче понять, какие поля есть у объекта, какие значения ожидает функция и какой результат она возвращает. Такой код проще читать и проверять в pull request.

## Темы модуля

- JavaScript и TypeScript: базовая разница;
- переменные: `let` и `const`;
- базовые типы: `string`, `number`, `boolean`;
- массивы;
- объекты;
- функции;
- параметры функций и тип возвращаемого значения;
- type inference;
- type annotations;
- `type`;
- `interface`;
- optional properties;
- union types на базовом уровне;
- чтение ошибок TypeScript;
- простые QA-ориентированные примеры.

## 1. JavaScript и TypeScript

JavaScript выполняется в средах вроде браузера или Node.js. Именно JavaScript в итоге запускается приложением или инструментом.

TypeScript — это расширение JavaScript с типами. TypeScript-код проверяется, а затем преобразуется в JavaScript. Благодаря проверке типов часть ошибок можно найти до запуска кода: например, когда в переменную с числом случайно передали строку или в объекте пропустили обязательное поле.

Для Automation QA это полезно, потому что ошибки в тестовых данных и вспомогательных функциях лучше увидеть сразу в редакторе, а не во время запуска тестов.

## 2. Переменные: let и const

`const` используют, когда значение не должно быть переопределено. `let` используют, когда значение может измениться. В современном TypeScript лучше начинать с `const` и переходить на `let` только тогда, когда переменную действительно нужно изменить.

`var` в современном TypeScript обычно не используют: у него менее предсказуемое поведение области видимости, и в новых проектах он почти всегда заменяется на `let` или `const`.

```ts id="g8ov7s"
const baseUrl = 'https://example.com';
let retryCount = 0;

retryCount = retryCount + 1;
```

QA-пример:

```ts id="0ccugq"
const userEmail = 'qa.user@example.com';
const expectedTitle = 'Dashboard';
let actualTitle = 'Login';
```

## 3. Базовые типы

На старте важно уверенно работать с тремя базовыми типами:

- `string` — строка, например email, заголовок страницы или сообщение об ошибке;
- `number` — число, например таймаут, код ответа или количество попыток;
- `boolean` — логическое значение `true` или `false`, например признак активного пользователя.

```ts id="su9e17"
const userName: string = 'Test User';
const userAge: number = 30;
const isActive: boolean = true;
```

Во многих случаях TypeScript сам понимает тип по значению. Это называется type inference.

```ts id="2m886u"
const browserName = 'chromium';
const timeout = 5000;
const isLoggedIn = false;
```

## 4. Массивы

Массив хранит список значений одного типа. В QA-коде массивы часто используются для списка браузеров, статусов, ролей, приоритетов или тестовых данных.

```ts id="f9vuhz"
const browsers: string[] = ['chromium', 'firefox', 'webkit'];
const statusCodes: number[] = [200, 201, 404, 500];
```

С массивом можно выполнять простые действия:

- читать элемент по индексу: `browsers[0]`;
- проверять длину: `browsers.length`;
- проходить по элементам через `for...of`.

```ts id="pajknn"
for (const browser of browsers) {
  console.log(browser);
}
```

## 5. Объекты

Объект хранит связанные поля: имена полей и значения. Это удобно для тестовых данных, потому что данные пользователя, тест-кейса или настройки можно держать вместе.

```ts id="4s5zgm"
const user = {
  email: 'qa.user@example.com',
  password: 'Password123!',
  isAdmin: false,
};
```

К полю объекта можно обратиться через точку:

```ts id="hhcymp"
console.log(user.email);
```

## 6. Функции

Функция — это переиспользуемая логика. Функция может получать параметры и возвращать значение. В тестовом коде функции помогают не копировать одинаковые действия и вычисления.

```ts id="hh4ljv"
function createUserEmail(prefix: string): string {
  return `${prefix}@example.com`;
}

const email = createUserEmail('qa.user');
```

QA-пример:

```ts id="42vr6a"
function isSuccessStatus(statusCode: number): boolean {
  return statusCode >= 200 && statusCode < 300;
}
```

## 7. Type annotations и type inference

Type annotation — это явное указание типа. Type inference означает, что TypeScript сам понимает тип по значению.

Не нужно добавлять типы везде, если TypeScript уже хорошо понимает код. Лучше добавлять типы там, где они улучшают читаемость: у параметров функций, возвращаемого значения функции или сложных объектов.

```ts id="shys1m"
const pageTitle = 'Login';
const maxRetries = 3;

function getExpectedMessage(userName: string): string {
  return `Welcome, ${userName}`;
}
```

## 8. type

`type` позволяет создать именованное описание данных. Это полезно для тестовых данных: можно один раз описать форму объекта и затем использовать её в разных местах.

```ts id="sgjyhh"
type TestUser = {
  email: string;
  password: string;
  isAdmin: boolean;
};

const adminUser: TestUser = {
  email: 'admin@example.com',
  password: 'Password123!',
  isAdmin: true,
};
```

## 9. interface

`interface` тоже описывает форму объекта. На этом этапе курса можно использовать и `type`, и `interface` для простых структур данных. Глубокие различия между ними пока не нужны: важно научиться читать и создавать понятные описания объектов.

```ts id="eyii9s"
interface LoginCredentials {
  email: string;
  password: string;
}

const credentials: LoginCredentials = {
  email: 'qa.user@example.com',
  password: 'Password123!',
};
```

## 10. Optional properties

Optional property — это необязательное поле. Оно может быть в объекте, а может отсутствовать. Символ `?` показывает, что поле необязательное.

```ts id="xr90ri"
type TestCase = {
  id: string;
  title: string;
  priority?: 'low' | 'medium' | 'high';
};
```

## 11. Union types

Union type означает, что переменная или поле может принимать одно из нескольких разрешённых значений. Это удобно для статусов, ролей и приоритетов: TypeScript не даст случайно написать значение, которого нет в списке.

```ts id="mu5nqa"
type TestStatus = 'passed' | 'failed' | 'skipped';

const status: TestStatus = 'passed';
```

QA-пример:

```ts id="e9g28k"
type UserRole = 'admin' | 'manager' | 'viewer';
```

## 12. Чтение ошибок TypeScript

Ошибки TypeScript — не враги. Чаще всего они показывают проблему до запуска кода. При ошибке важно спокойно прочитать сообщение, имя файла и номер строки. VS Code обычно подсвечивает место, где TypeScript видит проблему.

Пример намеренной ошибки:

```ts id="3ptnyk"
const retryCount: number = 'three';
```

Здесь переменная `retryCount` объявлена как `number`, но ей присвоена строка `'three'`. TypeScript подсказывает, что тип значения не совпадает с ожидаемым типом переменной.

## 13. Практические примеры для QA

Объект с данными пользователя:

```ts
const testUser = {
  email: 'qa.user@example.com',
  password: 'Password123!',
  role: 'viewer',
};
```

Объект с описанием тест-кейса:

```ts
type TestPriority = 'low' | 'medium' | 'high';

const loginTestCase = {
  id: 'LOGIN-001',
  title: 'Пользователь может войти с корректными данными',
  priority: 'high' as TestPriority,
};
```

Проверка успешного статуса:

```ts
function isSuccessStatus(statusCode: number): boolean {
  return statusCode >= 200 && statusCode < 300;
}
```

Генерация email:

```ts
function createUserEmail(prefix: string): string {
  return `${prefix}@example.com`;
}
```

Фильтрация тест-кейсов по приоритету:

```ts
type TestCase = {
  title: string;
  priority: 'low' | 'medium' | 'high';
};

const testCases: TestCase[] = [
  { title: 'Проверка входа', priority: 'high' },
  { title: 'Проверка подсказки', priority: 'low' },
];

const highPriorityTestCases = testCases.filter((testCase) => testCase.priority === 'high');
```

## Практика на занятии

1. Откройте домашний репозиторий в VS Code.
2. Переключитесь на ветку модуля 03.
3. Перед началом обновите ветку модуля 03 из личной ветки `master`.
4. Создайте TypeScript-файлы для упражнений.
5. Напишите переменные с базовыми типами.
6. Создайте массивы.
7. Создайте объекты для тестовых данных.
8. Напишите простые функции.
9. Создайте `type` и `interface`.
10. Запустите TypeScript typecheck.

```bash id="3twml4"
git fetch origin

git switch student/<student-name-slug>/master
git pull origin student/<student-name-slug>/master

git switch student/<student-name-slug>/module-03-typescript-basics
git merge origin/student/<student-name-slug>/master
git push
```

Затем запустите проверку типов:

```bash id="odg33n"
npm run typecheck
```

## Вопросы для проверки понимания

- Что такое TypeScript?
- Чем TypeScript на базовом уровне отличается от JavaScript?
- Почему TypeScript полезен для Automation QA?
- Что означает `const`?
- Что означает `let`?
- Почему `var` обычно не используют в современном TypeScript?
- Что такое `string`?
- Что такое `number`?
- Что такое `boolean`?
- Что такое массив?
- Что такое объект?
- Что такое функция?
- Что такое параметр функции?
- Что такое тип возвращаемого значения?
- Что такое type inference?
- Что такое type annotation?
- Что такое `type`?
- Что такое `interface`?
- Что такое optional property?
- Что такое union type?
- Почему важно внимательно читать ошибки TypeScript?

## Краткий итог

После модуля вы понимаете базовый синтаксис TypeScript, можете описывать простые данные с помощью типов, писать простые функции и запускать typecheck. Эти основы понадобятся дальше, когда вы будете писать и поддерживать Playwright-тесты.

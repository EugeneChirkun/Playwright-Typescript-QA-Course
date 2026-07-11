# Модуль 03: JavaScript и TypeScript — основы

## Цель модуля

Цель модуля — не выучить весь JavaScript и не разобрать весь TypeScript. На этом этапе важно получить минимальную базу, чтобы позже читать и писать простой код для автоматизации тестирования.

После модуля вы сможете:

- понимать простые TypeScript-файлы;
- создавать переменные;
- работать со строками, числами и boolean;
- создавать массивы и объекты;
- писать простые функции;
- использовать `if`;
- использовать `for...of`;
- понимать базовые type annotations;
- читать простые TypeScript errors.

## Важно: это не весь JavaScript и не весь TypeScript

Этот модуль даёт минимальную основу для Automation QA. Не нужно пытаться сразу запомнить весь язык и все возможности TypeScript.

- Более глубокий TypeScript для тестовых данных и helper-функций будет в модуле 04.
- `async/await` будет в модуле 05.
- ООП будет в модуле 06.
- Playwright будет изучаться позже, когда базовые конструкции кода станут понятнее.

## Почему это важно для Automation QA

Автоматизированные тесты — это код. Playwright-тесты часто пишут на TypeScript, поэтому Automation QA важно понимать базовые конструкции языка.

TypeScript помогает раньше находить ошибки: например, когда вместо числа случайно передали строку или забыли обязательное поле в тестовых данных. Типизированные тестовые данные проще читать, проверять на code review и поддерживать. Чем увереннее базовые знания JavaScript и TypeScript, тем легче будет перейти к Playwright.

## Темы модуля

- значения и переменные;
- `let` и `const`;
- строки, числа и boolean;
- выражения;
- массивы;
- объекты;
- функции;
- параметры и `return`;
- `if`;
- `for...of`;
- type inference;
- type annotations;
- простой `type`;
- простой `interface`;
- optional property;
- union type;
- TypeScript errors.

## 1. Значения и переменные

Значение — это конкретные данные: текст, число, `true` или `false`. Переменная — это именованное место, где хранится значение. Переменные нужны, чтобы не повторять одни и те же данные в коде и обращаться к ним по понятному имени.

```ts
const email = 'qa.user@example.com';
const retryCount = 3;
const isActive = true;
```

В этом примере `email` хранит строку с email, `retryCount` хранит число попыток, а `isActive` хранит boolean-значение и показывает, активен ли пользователь.

## 2. `const`, `let` и почему не `var`

`const` используют, когда значение не должно быть переопределено. `let` используют, когда значение может измениться. В современном TypeScript лучше избегать `var`, потому что у него менее понятные правила области видимости. По умолчанию выбирайте `const` и переходите на `let` только тогда, когда переменную действительно нужно менять.

```ts
const baseUrl = 'https://example.com';

let retryCount = 0;
retryCount = retryCount + 1;
```

QA-пример:

```ts
const userEmail = 'qa.user@example.com';
const expectedTitle = 'Dashboard';

let actualTitle = 'Login';
actualTitle = 'Dashboard';
```

## 3. Базовые типы: string, number, boolean

На старте чаще всего нужны три базовых типа:

- `string` — текст: email, заголовок страницы, сообщение об ошибке;
- `number` — число: timeout, status code, количество повторных попыток;
- `boolean` — значение `true` или `false`: включён ли режим, залогинен ли пользователь, активен ли флаг.

```ts
const userName: string = 'Test User';
const timeoutMs: number = 5000;
const isLoggedIn: boolean = false;
```

TypeScript часто сам понимает тип по значению. Это называется type inference.

```ts
const browserName = 'chromium';
const maxRetries = 3;
const isHeadless = true;
```

## 4. Простые выражения

Выражение — это часть кода, которая создаёт значение. Выражения используются в проверках, функциях и тестовых данных.

```ts
const retryCount = 2 + 1;
const fullName = 'Test' + ' ' + 'User';
const isSuccessful = 200 >= 200 && 200 < 300;
```

Для строк удобно использовать template string:

```ts
const userName = 'Alex';
const welcomeMessage = `Welcome, ${userName}`;
```

## 5. Массивы

Массив хранит несколько значений. В QA-коде массивы полезны для списков браузеров, ролей, статусов и тестовых данных.

```ts
const browsers: string[] = ['chromium', 'firefox', 'webkit'];
const statusCodes: number[] = [200, 201, 400, 500];
```

Элемент массива можно прочитать по индексу, а количество элементов — через `length`.

```ts
const firstBrowser = browsers[0];
const browsersCount = browsers.length;
```

## 6. Цикл `for...of`

Цикл помогает повторить одно действие для каждого элемента. `for...of` — простой способ пройти по массиву.

```ts
const browsers = ['chromium', 'firefox', 'webkit'];

for (const browser of browsers) {
  console.log(browser);
}
```

QA-пример:

```ts
const roles = ['admin', 'manager', 'viewer'];

for (const role of roles) {
  console.log(`Check permissions for role: ${role}`);
}
```

## 7. Объекты

Объект группирует связанные данные. В тестовом коде объекты часто используют для пользователя, тест-кейса, настроек или ожидаемого результата.

```ts
const user = {
  email: 'qa.user@example.com',
  password: 'Password123!',
  isAdmin: false,
};
```

К полям объекта обращаются через точку.

```ts
console.log(user.email);
console.log(user.isAdmin);
```

QA-пример:

```ts
const testCase = {
  id: 'TC-001',
  title: 'User can log in with valid credentials',
  priority: 'high',
};
```

## 8. Условия `if`

`if` позволяет выполнять код по-разному в зависимости от условия. Условие возвращает boolean-значение. Это полезно для проверок и helper-функций.

```ts
const statusCode = 200;

if (statusCode >= 200 && statusCode < 300) {
  console.log('Request was successful');
}
```

Пример с `else`:

```ts
const isLoggedIn = false;

if (isLoggedIn) {
  console.log('User is on dashboard');
} else {
  console.log('User should log in');
}
```

## 9. Функции

Функция — это переиспользуемая логика. Функция может получать параметры и возвращать значение. Функции помогают уменьшить дублирование кода.

```ts
function createUserEmail(prefix: string): string {
  return `${prefix}@example.com`;
}

const email = createUserEmail('qa.user');
```

QA-пример:

```ts
function isSuccessStatus(statusCode: number): boolean {
  return statusCode >= 200 && statusCode < 300;
}
```

Пример с `if`:

```ts
function getLoginMessage(isLoggedIn: boolean): string {
  if (isLoggedIn) {
    return 'User is logged in';
  }

  return 'User is not logged in';
}
```

## 10. Type annotations и type inference

TypeScript часто может определить тип автоматически. Это называется type inference. Явные type annotations полезны для параметров функций, возвращаемых значений и мест, где тип делает код понятнее.

Не нужно добавлять явные типы везде, если они очевидны из значения.

```ts
const pageTitle = 'Login';
const maxRetries = 3;

function getExpectedMessage(userName: string): string {
  return `Welcome, ${userName}`;
}
```

## 11. `type`: первое знакомство

`type` даёт имя форме данных или списку разрешённых значений. Это полезно для тестовых данных, потому что структура становится явной.

```ts
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

## 12. `interface`: первое знакомство

`interface` тоже описывает форму объекта. На этом этапе достаточно уметь читать и писать простые interfaces. Глубокие различия между `type` и `interface` сейчас не нужны.

```ts
interface LoginCredentials {
  email: string;
  password: string;
}

const credentials: LoginCredentials = {
  email: 'qa.user@example.com',
  password: 'Password123!',
};
```

## 13. Optional property

Optional property — это поле, которого может не быть в объекте. Символ `?` показывает, что поле необязательное.

```ts
interface TestCase {
  id: string;
  title: string;
  description?: string;
}
```

## 14. Union type

Union type ограничивает значение несколькими разрешёнными вариантами. Это удобно для ролей, статусов и приоритетов.

```ts
type UserRole = 'admin' | 'manager' | 'viewer';
type TestPriority = 'low' | 'medium' | 'high';

const role: UserRole = 'admin';
const priority: TestPriority = 'high';
```

## 15. Как читать TypeScript errors

TypeScript error обычно показывает файл, строку и причину проблемы. Сначала внимательно прочитайте сообщение, затем посмотрите на подсветку в VS Code. Ошибка до запуска кода полезна: её можно исправить раньше, чем она попадёт в тестовый запуск.

```ts
const retryCount: number = 'three';
```

Здесь `retryCount` объявлен как `number`, но значение `'three'` — это строка. TypeScript подсказывает, что тип значения не совпадает с ожидаемым типом переменной.

## Что обязательно понять в этом модуле

- переменные;
- базовые типы;
- массивы;
- объекты;
- функции;
- `if`;
- `for...of`;
- чтение TypeScript errors.

## Что пока достаточно узнать обзорно

- `type`;
- `interface`;
- optional properties;
- union types.

Эти темы будут практиковаться глубже в модуле 04, когда появится больше QA-сценариев с тестовыми данными и helper-функциями.

## Практика на занятии

1. Открыть homework repository в VS Code.
2. Проверить текущую ветку.
3. Переключиться на личную ветку `master` и подтянуть обновления.
4. Переключиться на ветку модуля 03.
5. Перед работой выполнить merge личной ветки `master` в ветку модуля 03.
6. Создать файлы с упражнениями.
7. Написать простые переменные.
8. Создать массивы.
9. Создать объекты.
10. Добавить `for...of`.
11. Добавить `if`.
12. Написать простые функции.
13. Добавить первый `type` и первый `interface`.
14. Запустить `npm run typecheck`.

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-03-typescript-basics
git merge origin/student/{student-name-slug}/master
git push
```

Затем запустите проверку типов:

```bash
npm run typecheck
```

## Вопросы для проверки понимания

- Что такое переменная?
- Чем значение отличается от переменной?
- Что такое `const`?
- Что такое `let`?
- Почему `var` обычно избегают в современном коде?
- Что такое `string`?
- Что такое `number`?
- Что такое `boolean`?
- Что такое массив?
- Что такое объект?
- Для чего нужен `if`?
- Для чего нужен `for...of`?
- Что такое функция?
- Что такое параметр функции?
- Что делает `return`?
- Что такое type inference?
- Что такое type annotation?
- Для чего используют `type`?
- Для чего используют `interface`?
- Что такое optional property?
- Что такое union type?
- Почему TypeScript errors полезны?

## Краткий итог

В этом модуле вы получили минимальную JavaScript- и TypeScript-основу для дальнейшего движения к Automation QA. Теперь вы можете писать простой типизированный код, читать базовые TypeScript errors и понимать основные конструкции, которые позже встретятся в Playwright-тестах. Более глубокая работа с TypeScript для QA продолжится в модуле 04.

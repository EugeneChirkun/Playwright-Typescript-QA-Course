# Модуль 05: Async/await и Promise

## Цель модуля

Цель модуля — понять асинхронный код на уровне, необходимом для работы с Playwright и автоматизации тестирования. Материал опирается на основы JavaScript и TypeScript из модуля 03 и на типизацию QA-данных из модуля 04.

После модуля вы сможете:

- объяснить, что такое асинхронный код;
- понимать, что означает `Promise`;
- писать простые `async`-функции и использовать `await`;
- избегать частой ошибки с пропущенным `await`;
- обрабатывать ошибки с помощью `try/catch`;
- определять, когда операции выполняются последовательно, а когда могут выполняться параллельно;
- читать простой асинхронный код на TypeScript.

## Почему это важно для Automation QA

Действия в интерфейсе, переходы между страницами, ожидание элементов, API-вызовы и операции с файлами могут быть асинхронными. Playwright активно использует Promise, поэтому позднее `await` будет встречаться почти в каждом тесте. Пропущенный `await` может нарушить порядок шагов, привести к нестабильным и непредсказуемым тестам. Понимание асинхронности помогает находить причину таких сбоев.

## Темы модуля

- синхронный и асинхронный код;
- `Promise` и состояния pending, fulfilled, rejected;
- `async`-функция и `await`;
- последовательное выполнение;
- частая ошибка: пропущенный `await`;
- `try/catch` и простая обработка ошибок;
- вводное знакомство с `Promise.all`;
- практические примеры в стиле QA-задач.

## 1. Синхронный и асинхронный код

Синхронный код выполняется шаг за шагом: следующая инструкция начинается после предыдущей.

```ts
console.log('Step 1');
console.log('Step 2');
console.log('Step 3');
```

Асинхронный код запускает операцию, а её результат становится доступен позднее. Такой подход полезен, когда операция требует времени и программа не должна бессмысленно блокировать остальную работу.

```ts
console.log('Start');

setTimeout(() => {
  console.log('Finished later');
}, 1000);

console.log('End');
```

Сначала будут выведены `Start` и `End`, а затем — `Finished later`. Здесь `setTimeout` нужен только для простой демонстрации. Это не основной инструмент ожидания в тестах.

## 2. Что такое Promise

`Promise` — объект, представляющий значение, которое может стать доступно позднее. Его удобно воспринимать как обещание вернуть результат асинхронной операции.

У Promise есть три состояния:

- `pending` — операция ещё выполняется;
- `fulfilled` — операция успешно завершена;
- `rejected` — операция завершилась ошибкой.

```ts
function waitForMessage(): Promise<string> {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('Message is ready');
    }, 1000);
  });
}
```

Функция сразу возвращает `Promise<string>`. Через секунду вызов `resolve` успешно завершает Promise строкой.

## 3. `async`-функция

Ключевое слово `async` отмечает функцию как асинхронную. Такая функция всегда возвращает Promise. Даже простое значение автоматически оборачивается в Promise.

```ts
async function getUserName(): Promise<string> {
  return 'Test User';
}
```

Результат такой функции обычно получают с помощью `await`.

## 4. `await`

`await` ожидает успешного завершения Promise и возвращает его итоговое значение. В рассматриваемых примерах `await` используется внутри `async`-функции. Без `await` переменная получит сам Promise, а не готовое значение.

```ts
async function printUserName(): Promise<void> {
  const userName = await getUserName();

  console.log(userName);
}
```

## 5. Пропущенный `await`: частая ошибка

Пропущенный `await` — одна из самых частых ошибок в асинхронном TypeScript и Playwright.

Неверный вариант:

```ts
async function badExample(): Promise<void> {
  const userNamePromise = getUserName();

  console.log(userNamePromise);
}
```

Верный вариант:

```ts
async function goodExample(): Promise<void> {
  const userName = await getUserName();

  console.log(userName);
}
```

В первом примере в переменной хранится `Promise<string>`. Во втором после ожидания хранится готовая строка.

## 6. Последовательное выполнение

Если каждый шаг зависит от предыдущего или порядок действий важен, используйте последовательные вызовы `await`.

```ts
async function getUserEmail(): Promise<string> {
  return 'qa.user@example.com';
}

async function getUserPassword(): Promise<string> {
  return 'Password123!';
}
```

```ts
async function loginFlow(): Promise<void> {
  const email = await getUserEmail();
  const password = await getUserPassword();

  console.log(`Login with ${email} and ${password}`);
}
```

Это похоже на реальные шаги теста: открыть страницу, выполнить действие и проверить результат. Там порядок имеет значение.

## 7. Параллельное выполнение и `Promise.all`

Иногда операции не зависят друг от друга. Тогда их можно запустить вместе, а `Promise.all` дождётся завершения всех переданных Promise.

```ts
async function loadTestData(): Promise<void> {
  const [email, password] = await Promise.all([
    getUserEmail(),
    getUserPassword(),
  ]);

  console.log(email);
  console.log(password);
}
```

Используйте `Promise.all` только тогда, когда операции действительно независимы. Если второй шаг использует результат первого, выполняйте их последовательно. Сложные варианты параллельного выполнения пока не нужны.

## 8. Ошибки и `try/catch`

Асинхронная операция может завершиться ошибкой, то есть вернуть rejected Promise. `try/catch` позволяет явно обработать такую ситуацию.

```ts
async function getUserFromService(): Promise<string> {
  throw new Error('User service is not available');
}

async function printUserFromService(): Promise<void> {
  try {
    const user = await getUserFromService();

    console.log(user);
  } catch (error) {
    console.log('Could not get user');
    console.log(error);
  }
}
```

В блоке `try` находится код, который может завершиться ошибкой. Блок `catch` получает ошибку и определяет, как на неё отреагировать.

## 9. Возвращаемые типы асинхронных функций

Асинхронная функция возвращает `Promise<T>`, где `T` — тип значения после `await`. Например, `Promise<string>` означает «позднее вернётся строка», а `Promise<void>` — «операция завершится без полезного возвращаемого значения».

```ts
async function getStatusCode(): Promise<number> {
  return 200;
}

async function isUserActive(): Promise<boolean> {
  return true;
}

async function doNothing(): Promise<void> {
  console.log('Done');
}
```

## 10. Асинхронные примеры для QA

Асинхронные функции могут загружать данные пользователя, получать код состояния, подготавливать тестовые данные и форматировать полученный результат.

```ts
type TestUser = {
  email: string;
  password: string;
  isActive: boolean;
};

async function getTestUser(): Promise<TestUser> {
  return {
    email: 'qa.user@example.com',
    password: 'Password123!',
    isActive: true,
  };
}

async function printUserStatus(): Promise<void> {
  const user = await getTestUser();

  if (user.isActive) {
    console.log(`${user.email} is active`);
  }
}
```

Здесь `getTestUser` подготавливает данные, а `printUserStatus` ожидает результат, проверяет статус и форматирует сообщение.

## 11. Как это связано с Playwright

Многие методы Playwright асинхронны и возвращают Promise. Позднее код будет содержать вызовы `await page.goto(...)`, `await page.click(...)` и асинхронные проверки с `await expect(...)`. Сейчас важно понять, почему перед такими действиями нужен `await`.

```ts
// Preview for later modules
await page.goto('https://example.com');
await page.getByRole('button', { name: 'Login' }).click();
```

Это только предварительный пример. Locators, действия и проверки Playwright будут подробно рассмотрены в следующих модулях.

## 12. Типичные ошибки

- забыть `await` и работать с Promise вместо значения;
- использовать `await` вне подходящего асинхронного контекста;
- не вернуть ожидаемое значение из `async`-функции;
- перехватывать ошибки слишком широко, не выясняя их причину;
- применять `Promise.all`, когда шаги зависят друг от друга;
- игнорировать возвращаемые типы TypeScript.

## Что обязательно понять в этом модуле

- что такое Promise;
- что такое `async`-функция;
- что делает `await`;
- чем опасен пропущенный `await`;
- как выглядит последовательный асинхронный код;
- как применить базовый `try/catch`.

## Что пока достаточно узнать обзорно

- состояния Promise;
- назначение `Promise.all`;
- возвращаемый тип `Promise<T>`.

## Практика на занятии

Практика выполняется в отдельном репозитории домашних заданий:

1. Откройте репозиторий домашних заданий в VS Code.
2. Проверьте текущую ветку.
3. Переключитесь на личную ветку `master` и получите обновления.
4. Переключитесь на ветку модуля 05.
5. До начала работы влейте личную ветку `master` в ветку модуля.
6. Создайте файлы с упражнениями по асинхронности.
7. Напишите простой Promise.
8. Напишите `async`-функции.
9. Получите результат с помощью `await`.
10. Исправьте пример с пропущенным `await`.
11. Добавьте обработку ошибки через `try/catch`.
12. Запустите typecheck.

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-05-async-await-promises
git merge origin/student/{student-name-slug}/master
git push
```

```bash
npm run typecheck
```

## Вопросы для проверки понимания

1. Что такое синхронный код?
2. Что такое асинхронный код?
3. Почему асинхронность важна в автоматизации тестирования?
4. Что представляет собой Promise?
5. Что означают состояния pending, fulfilled и rejected?
6. Что означает ключевое слово `async` перед функцией?
7. Что делает `await`?
8. Где можно использовать `await` в примерах этого модуля?
9. Что произойдёт, если забыть `await`?
10. Почему `async`-функция возвращает `Promise<T>`?
11. Что означает тип `Promise<string>`?
12. Что означает тип `Promise<void>`?
13. Что такое последовательное выполнение асинхронных операций?
14. В каком случае полезен `Promise.all`?
15. Почему с `Promise.all` нужно быть осторожным?
16. Для чего используется `try/catch`?
17. Как rejected Promise связан с обработкой ошибок?
18. Как async/await связан с будущими тестами на Playwright?

## Краткий итог

Вы познакомились с основами асинхронности, можете написать `async`-функцию, получить результат через `await` и обработать ошибку. Понимание проблемы пропущенного `await` и порядка операций подготовило вас к дальнейшей работе с Playwright.

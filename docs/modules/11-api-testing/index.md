# Модуль 11: API testing в Playwright

## Главное в модуле

- HTTP-запросы через Playwright;
- проверки status code, headers и тела ответа;
- типизированные данные запроса и ответа.

## Минимальный результат

После модуля студент должен уметь:

- отправить безопасный запрос к учебному API;
- проверить ключевые свойства ответа;
- объяснить ограничения демонстрационного API.


## Цель модуля

Цель модуля — научиться писать базовые API-тесты с помощью Playwright и понимать, как проверки API поддерживают автоматизацию UI.

После модуля вы сможете:

- объяснить, что такое API;
- понимать базовую структуру HTTP request и response;
- использовать fixture `request` в Playwright;
- отправлять запросы `GET`, `POST`, `PUT`, `PATCH` и `DELETE`;
- проверять status code, response body и headers;
- работать с типизированными данными response;
- подготавливать данные через API, когда это уместно;
- объяснить, как API- и UI-тесты дополняют друг друга.

Материал опирается на TypeScript, `async`/`await`, ООП и возможности Playwright из модулей 03–10.

## Почему API testing важен для Automation QA

API-тесты обычно выполняются быстрее UI-тестов и позволяют проверять бизнес-логику ниже уровня интерфейса. Через API удобно создавать и удалять тестовые данные. Благодаря этому UI-тесты становятся короче и стабильнее: интерфейс проверяет только то, что действительно относится к нему. В работе Automation QA часто нужны оба уровня тестирования.

## Темы модуля

- назначение API;
- HTTP request и response;
- status code и headers;
- request body, response body и JSON;
- методы `GET`, `POST`, `PUT`, `PATCH` и `DELETE`;
- fixture `request` и `APIRequestContext`;
- проверки API response;
- типизированные данные API;
- вспомогательные функции для API;
- совместное использование UI и API;
- типичные ошибки.

## 1. Что такое API

API — это способ взаимодействия программных систем. В веб-приложении интерфейс часто обращается к API серверной части.

Например:

1. Пользователь нажимает «Сохранить».
2. Браузер отправляет request к API.
3. Серверная часть обрабатывает данные.
4. API возвращает response.
5. Интерфейс показывает результат.

API-тест проверяет это взаимодействие напрямую, без действий в браузерном интерфейсе.

## 2. Request и response

Request — это запрос, который клиент отправляет серверу. Он может содержать:

- HTTP method;
- URL;
- headers;
- body.

Response — это ответ сервера. Он может содержать:

- status code;
- headers;
- body.

```text
GET /users/1

Response:
Status: 200
Body:
{
  "id": 1,
  "email": "qa.user@example.com"
}
```

## 3. HTTP methods

- `GET` получает данные, например пользователя по идентификатору.
- `POST` создает данные, например нового пользователя.
- `PUT` обычно полностью заменяет или обновляет объект.
- `PATCH` обычно изменяет часть объекта, например только роль пользователя.
- `DELETE` удаляет данные, например тестовую запись.

Назначение конкретного метода и формат данных определяет API тестируемого приложения.

## 4. Status codes

Часто встречаются следующие status codes:

- `200 OK` — запрос успешно обработан;
- `201 Created` — ресурс успешно создан;
- `204 No Content` — запрос выполнен, response body отсутствует;
- `400 Bad Request` — запрос составлен неверно;
- `401 Unauthorized` — требуется аутентификация;
- `403 Forbidden` — доступ к действию запрещен;
- `404 Not Found` — ресурс не найден;
- `500 Internal Server Error` — внутренняя ошибка сервера.

Одного status code не всегда достаточно. Успешный код может сопровождать неверные данные, поэтому обычно проверяют и response body.

## 5. JSON response body

Многие API возвращают JSON. Он похож на объект JavaScript, но является форматом данных.

```json
{
  "id": 1,
  "email": "qa.user@example.com",
  "role": "viewer",
  "isActive": true
}
```

В API-тесте Playwright JSON обычно читают асинхронно:

```ts
const body = await response.json();
```

## 6. Первый API-тест с Playwright

Для учебных примеров используем публичный демонстрационный API JSONPlaceholder.

```ts
import { test, expect } from '@playwright/test';

test('get post by id', async ({ request }) => {
  const response = await request.get('https://jsonplaceholder.typicode.com/posts/1');

  expect(response.status()).toBe(200);

  const body = await response.json();

  expect(body.id).toBe(1);
});
```

`request` — встроенная fixture Playwright. Метод `request.get()` отправляет GET request, `response.status()` возвращает status code, а `response.json()` читает JSON из response body.

## 7. Проверка response body

```ts
import { test, expect } from '@playwright/test';

test('post has expected fields', async ({ request }) => {
  const response = await request.get('https://jsonplaceholder.typicode.com/posts/1');
  const body = await response.json();

  expect(response.ok()).toBeTruthy();
  expect(body).toHaveProperty('id');
  expect(body).toHaveProperty('title');
  expect(body).toHaveProperty('body');
  expect(body.userId).toBe(1);
});
```

`response.ok()` возвращает `true` для успешных status codes из диапазона 2xx. Поля body можно проверять знакомыми assertions Playwright.

## 8. Проверка headers

```ts
import { test, expect } from '@playwright/test';

test('response has content type header', async ({ request }) => {
  const response = await request.get('https://jsonplaceholder.typicode.com/posts/1');

  expect(response.headers()['content-type']).toContain('application/json');
});
```

Headers содержат технические метаданные: формат response, сведения об авторизации, кешировании и другие параметры.

## 9. POST request

```ts
import { test, expect } from '@playwright/test';

test('create post', async ({ request }) => {
  const response = await request.post('https://jsonplaceholder.typicode.com/posts', {
    data: {
      title: 'QA test title',
      body: 'QA test body',
      userId: 1,
    },
  });

  expect(response.status()).toBe(201);

  const body = await response.json();

  expect(body.title).toBe('QA test title');
  expect(body.body).toBe('QA test body');
  expect(body.userId).toBe(1);
});
```

Параметр `data` задает request body. `POST` обычно создает ресурс, но демонстрационный API может лишь имитировать создание и не сохранять данные.

## 10. PUT, PATCH и DELETE

`PUT` обычно отправляет полный обновленный объект, а `PATCH` — только изменяемые поля.

```ts
import { test, expect } from '@playwright/test';

test('update post with PUT', async ({ request }) => {
  const response = await request.put('https://jsonplaceholder.typicode.com/posts/1', {
    data: {
      id: 1,
      title: 'Updated title',
      body: 'Updated body',
      userId: 1,
    },
  });

  expect(response.status()).toBe(200);

  const body = await response.json();

  expect(body.title).toBe('Updated title');
});
```

Для частичного обновления можно вызвать `request.patch()` и передать в `data` только нужные поля.

```ts
import { test, expect } from '@playwright/test';

test('delete post', async ({ request }) => {
  const response = await request.delete('https://jsonplaceholder.typicode.com/posts/1');

  expect(response.status()).toBe(200);
});
```

## 11. Типизированный API response

Типы из модуля 04 помогают видеть ожидаемую структуру данных.

```ts
import { test, expect } from '@playwright/test';

type PostResponse = {
  userId: number;
  id: number;
  title: string;
  body: string;
};

test('get typed post response', async ({ request }) => {
  const response = await request.get('https://jsonplaceholder.typicode.com/posts/1');
  const body = await response.json() as PostResponse;

  expect(body.id).toBe(1);
  expect(body.userId).toBe(1);
});
```

Type помогает читать структуру response и получать подсказки редактора. Однако type assertion не проверяет фактические данные во время выполнения: для этого по-прежнему нужны assertions.

## 12. Вспомогательные функции для API

Повторяющиеся запросы полезно выносить во вспомогательные функции.

```ts
import { APIRequestContext, expect } from '@playwright/test';

export type PostResponse = {
  userId: number;
  id: number;
  title: string;
  body: string;
};

export async function getPostById(
  request: APIRequestContext,
  postId: number,
): Promise<PostResponse> {
  const response = await request.get(`https://jsonplaceholder.typicode.com/posts/${postId}`);

  expect(response.status()).toBe(200);

  return await response.json() as PostResponse;
}
```

```ts
import { test, expect } from '@playwright/test';
import { getPostById } from '../../src/api/posts.api';

test('get post through helper', async ({ request }) => {
  const post = await getPostById(request, 1);

  expect(post.id).toBe(1);
});
```

Вспомогательная функция скрывает повторяющийся код request, а тест остается сосредоточен на сценарии. При этом не стоит скрывать в ней слишком много важных для сценария assertions.

## 13. Тестовые данные для API

Request data также можно типизировать, чтобы уменьшить количество ошибок в названиях и типах полей.

```ts
export type CreatePostRequest = {
  title: string;
  body: string;
  userId: number;
};

export const newPostData: CreatePostRequest = {
  title: 'QA title',
  body: 'QA body',
  userId: 1,
};
```

## 14. Совместное использование API и UI

API может поддерживать UI-тесты:

- создать данные через API, а затем проверить их в UI;
- выполнить действие в UI и через API проверить изменение данных;
- удалить тестовые данные через API после теста.

```ts
import { test } from '@playwright/test';

test('UI and API can support each other', async ({ page, request }) => {
  const apiResponse = await request.get('https://jsonplaceholder.typicode.com/posts/1');
  const post = await apiResponse.json();

  await page.goto('/');

  console.log(post.title);
});
```

Это только простая демонстрация совместного использования fixtures. Реальная интеграция UI и API зависит от тестируемого приложения, его данных и доступных endpoints.

## 15. Аутентификация в API-тестах

Как обсуждалось в модуле 10, доступ может зависеть от состояния аутентификации. API может ожидать token, cookie, session или специальный header.

```ts
await request.get('/api/user', {
  headers: {
    Authorization: `Bearer ${token}`,
  },
});
```

В этом модуле достаточно понимать назначение authorization header. Сложные схемы и внутреннее устройство OAuth здесь не рассматриваются. Реальные tokens и пароли нельзя добавлять в код и Git.

## 16. Типичные ошибки

- Проверять только status code и игнорировать response body.
- Считать, что `response.json()` автоматически проверяет данные.
- Забывать `await` перед асинхронным вызовом.
- Повторять одинаковые URL во всех тестах.
- Использовать реальные производственные данные.
- Добавлять реальные пароли или tokens в код.
- Не отделять request data от тестовых сценариев.
- Делать вспомогательные функции API слишком сложными.
- Проверять через UI то, что быстрее и надежнее проверить через API.
- Ожидать сохранения изменений от демонстрационного API, который лишь имитирует операции.

## Что обязательно понять в этом модуле

- Основы API, request и response.
- Назначение status code и response body.
- Методы `GET` и `POST`.
- Fixture `request` в Playwright.
- Чтение JSON через `await response.json()`.
- Базовые assertions для API.
- Типизированные данные response.

## Что пока достаточно узнать обзорно

- Методы `PUT`, `PATCH` и `DELETE`.
- Вспомогательные функции API.
- Совместное использование UI и API.
- Authorization headers.

## Практика на занятии

Практика выполняется в отдельном репозитории домашних заданий.

1. Откройте репозиторий домашних заданий в VS Code.
2. Проверьте текущую ветку.
3. Переключитесь на личную ветку `master` и получите обновления.
4. Переключитесь на ветку модуля 11.
5. Перед началом работы добавьте в нее изменения из личной ветки `master`.
6. Создайте файлы API-тестов.
7. Напишите тест с `GET`.
8. Напишите тест с `POST`.
9. Проверьте status code.
10. Проверьте response body.
11. Добавьте типизированные данные response.
12. Создайте простую вспомогательную функцию API.
13. Запустите typecheck.
14. Запустите API-тесты.

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-11-api-testing
git merge origin/student/{student-name-slug}/master
git push
```

```bash
npm run typecheck
npm run test
```

## Вопросы для проверки понимания

1. Что такое API?
2. Почему API testing полезен Automation QA?
3. Что такое HTTP request?
4. Что такое HTTP response?
5. Что такое status code?
6. Что означает status code `200`?
7. Что означает status code `201`?
8. Что означает status code `404`?
9. Что такое request body?
10. Что такое response body?
11. Что такое JSON?
12. Для чего используется `GET`?
13. Для чего используется `POST`?
14. Для чего используется `DELETE`?
15. Что предоставляет fixture `request` в Playwright?
16. Что делает `response.json()`?
17. Почему проверки только status code часто недостаточно?
18. Чем полезны типизированные данные response?
19. Чего следует избегать при работе с реальными учетными данными и tokens?
20. Как API может поддержать UI-тесты?

## Краткий итог

Вы познакомились с основами API testing, научились писать простые API-тесты с Playwright и проверять status code, body и headers. Вы умеете описывать API response через TypeScript type и понимаете, как API помогает подготавливать данные и поддерживать UI-тесты.

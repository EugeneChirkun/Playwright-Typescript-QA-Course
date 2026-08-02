# Домашнее задание: Модуль 11

## Цель домашнего задания

Цель — закрепить API testing с Playwright на практике в отдельном репозитории домашних заданий.

Вам предстоит:

- перед началом обновить ветку модуля 11 из личной ветки `master`;
- создать файлы API-тестов;
- создать простые вспомогательные функции API;
- описать типизированные request и response data;
- заполнить `homework/module-11-api-testing/result.md`;
- запустить `npm run typecheck` и API-тесты;
- открыть PR в личную ветку `master`.

## Задание 1. Подготовить ветку модуля 11

В репозитории домашних заданий выполните:

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-11-api-testing
git merge origin/student/{student-name-slug}/master
git push
```

## Задание 2. Создать файлы

Создайте или обновите следующую структуру. Если каких-либо каталогов нет, создайте их.

```text
src/api/posts.api.ts
src/test-data/posts.ts
tests/module-11/api-basics.spec.ts
tests/module-11/posts-api.spec.ts
homework/module-11-api-testing/result.md
```

Для учебных запросов используйте `https://jsonplaceholder.typicode.com`.

## Задание 3. Подготовить тестовые данные API

В `src/test-data/posts.ts` создайте и экспортируйте:

- type `CreatePostRequest`;
- type `PostResponse`;
- объект `newPostData`.

Используйте поля `title`, `body` и `userId`. Для response также предусмотрите `id`.

## Задание 4. Написать базовые API-тесты

В `tests/module-11/api-basics.spec.ts` с помощью fixture `request` создайте тесты, которые:

- получают публикацию по идентификатору через `GET`;
- проверяют status code;
- проверяют поля response body;
- проверяют header `content-type`;
- отправляют `POST` для новой публикации;
- проверяют response созданной публикации.

## Задание 5. Создать вспомогательные функции API

В `src/api/posts.api.ts` создайте функции:

- `getPostById(request: APIRequestContext, postId: number): Promise<PostResponse>`;
- `createPost(request: APIRequestContext, data: CreatePostRequest): Promise<PostResponse>`;
- по желанию — `deletePost(request: APIRequestContext, postId: number): Promise<void>`.

Импортируйте `APIRequestContext` из Playwright. Повторяющийся код запроса разместите внутри функций, но оставьте важные для сценария проверки в тесте.

## Задание 6. Написать тесты со вспомогательными функциями

В `tests/module-11/posts-api.spec.ts` создайте читаемые тесты, которые:

- получают публикацию через `getPostById()`;
- создают публикацию через `createPost()`;
- по желанию удаляют публикацию через `deletePost()`.

## Задание 7. Запустить typecheck и тесты

```bash
npm run typecheck
npm run test
```

Если тест не проходит из-за особенностей публичного демонстрационного API или отсутствия реального сохранения данных, опишите это в `result.md`. Проверка TypeScript при этом должна проходить.

## Задание 8. Заполнить result.md

В `homework/module-11-api-testing/result.md` укажите:

- название текущей ветки;
- созданные или обновленные файлы;
- выполненные команды;
- результат `npm run typecheck`;
- результат запуска API-тестов;
- краткое объяснение API;
- краткое объяснение request и response;
- краткое объяснение status code;
- краткое объяснение response body;
- краткое объяснение fixture `request` в Playwright;
- краткое объяснение типизированного API response;
- возникшие вопросы или проблемы.

## Задание 9. Создать commit, отправить изменения и открыть PR

```bash
git status
git add .
git commit -m "Complete module 11 API testing homework"
git push
```

Для PR выберите:

```text
base: student/{student-name-slug}/master
compare: student/{student-name-slug}/module-11-api-testing
```

## Ожидаемый результат

- [ ] Ветка модуля 11 обновлена из личной ветки `master` до начала работы.
- [ ] API-тесты созданы.
- [ ] Вспомогательные функции API созданы.
- [ ] Типизированные request и response data созданы.
- [ ] `result.md` заполнен.
- [ ] `npm run typecheck` проходит.
- [ ] API-тесты запущены, результат зафиксирован.
- [ ] Изменения добавлены в commit и отправлены в удаленный репозиторий.
- [ ] PR открыт в личную ветку `master`.
- [ ] `node_modules` и сгенерированные отчеты не добавлены в Git.
- [ ] Реальные учетные данные и tokens не добавлены в Git.
- [ ] В коде и документации нет реальных имен студентов.

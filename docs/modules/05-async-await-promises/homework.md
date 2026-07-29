# Домашнее задание: Модуль 05

## Цель домашнего задания

Цель — закрепить async/await и Promise на небольших примерах TypeScript. Работа выполняется в отдельном репозитории домашних заданий, а не в репозитории документации курса.

Перед началом нужно обновить ветку модуля 05 из личной ветки `master`. Затем необходимо создать TypeScript-файлы в `src/training/module-05-async-await-promises/`, заполнить `homework/module-05-async-await-promises/result.md`, запустить `npm run typecheck` и открыть PR в личную ветку `master`.

## Задание 1. Подготовить ветку модуля 05

Получите последние изменения и влейте личную ветку `master` в ветку модуля до начала работы:

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-05-async-await-promises
git merge origin/student/{student-name-slug}/master
git push
```

## Задание 2. Создать файлы для упражнений

Создайте каталог:

```text
src/training/module-05-async-await-promises/
```

Подготовьте файлы:

```text
src/training/module-05-async-await-promises/01-promises.ts
src/training/module-05-async-await-promises/02-async-await.ts
src/training/module-05-async-await-promises/03-missing-await.ts
src/training/module-05-async-await-promises/04-try-catch.ts
src/training/module-05-async-await-promises/05-promise-all.ts
src/training/module-05-async-await-promises/index.ts
homework/module-05-async-await-promises/result.md
```

## Задание 3. Основы Promise

В `01-promises.ts` создайте:

- `waitForMessage(): Promise<string>`;
- `getStatusCode(): Promise<number>`;
- `isFeatureEnabled(): Promise<boolean>`.

Каждая функция должна возвращать Promise. Для имитации задержки можно использовать `setTimeout`:

```ts
function waitForMessage(): Promise<string> {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('Message is ready');
    }, 500);
  });
}
```

## Задание 4. async/await

В `02-async-await.ts` создайте:

- `getUserEmail(): Promise<string>`;
- `getUserPassword(): Promise<string>`;
- `printLoginData(): Promise<void>`.

Внутри `printLoginData` получите оба значения с помощью `await` и выведите их в консоль.

## Задание 5. Пропущенный `await`

В `03-missing-await.ts` добавьте один неверный пример с пропущенным `await` и один исправленный пример с `await`. Если неверный пример нарушает typecheck, оставьте его закомментированным.

В `result.md` объясните своими словами, почему пропущенный `await` опасен для автоматизированных тестов.

## Задание 6. try/catch

В `04-try-catch.ts` создайте:

- асинхронную функцию, которая выбрасывает ошибку;
- функцию, которая вызывает её внутри `try/catch`;
- понятные сообщения для успешного и ошибочного исходов.

```ts
async function getUserFromService(): Promise<string> {
  throw new Error('User service is not available');
}

async function checkUserService(): Promise<void> {
  try {
    const user = await getUserFromService();

    console.log(user);
  } catch (error) {
    console.log('Could not get user from service');
    console.log(error);
  }
}
```

## Задание 7. Promise.all

В `05-promise-all.ts` создайте:

- `getUserEmail(): Promise<string>`;
- `getUserRole(): Promise<string>`;
- `loadUserData(): Promise<void>`.

Внутри `loadUserData` используйте `Promise.all`. В комментарии поясните, что получение электронной почты и роли не зависит друг от друга, поэтому здесь допустим `Promise.all`.

## Задание 8. Подготовить index.ts

В `index.ts` импортируйте или вызовите несколько функций из файлов с упражнениями. Не усложняйте точку входа и убедитесь, что typecheck проходит.

Если работа с импортами и экспортами пока вызывает затруднения, можно оставить примеры независимыми. В любом случае `npm run typecheck` должен завершаться без ошибок.

## Задание 9. Запустить typecheck

```bash
npm run typecheck
```

Исправьте все ошибки TypeScript до создания PR.

## Задание 10. Заполнить result.md

В `homework/module-05-async-await-promises/result.md` укажите:

- текущую ветку;
- созданные файлы;
- выполненные команды;
- прошёл ли `npm run typecheck`;
- краткое объяснение Promise;
- краткое объяснение `async`-функции;
- краткое объяснение `await`;
- объяснение проблемы пропущенного `await`;
- краткое объяснение `try/catch`;
- краткое объяснение `Promise.all`;
- возникшие вопросы или проблемы.

## Задание 11. Создать commit, отправить изменения и открыть PR

```bash
git status
git add .
git commit -m "Complete module 05 async await promises homework"
git push
```

При создании PR выберите:

```text
base: student/{student-name-slug}/master
compare: student/{student-name-slug}/module-05-async-await-promises
```

## Ожидаемый результат

- [ ] Ветка модуля 05 обновлена из личной ветки `master` до начала работы.
- [ ] TypeScript-файлы созданы в `src/training/module-05-async-await-promises/`.
- [ ] Файл `result.md` заполнен.
- [ ] `npm run typecheck` проходит без ошибок.
- [ ] Изменения сохранены в commit и отправлены в удалённый репозиторий.
- [ ] PR открыт в личную ветку `master`.
- [ ] `node_modules` не добавлен в commit.
- [ ] Созданные отчёты не добавлены в commit.
- [ ] В коде и документации не используются реальные имена учащихся.

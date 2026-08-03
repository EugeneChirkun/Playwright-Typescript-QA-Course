# Модуль 08: Локаторы, проверки и auto-waiting

## Главное в модуле

- локаторы, ориентированные на пользователя;
- web-first проверки Playwright;
- auto-waiting вместо фиксированных задержек.

## Минимальный результат

После модуля студент должен уметь:

- выбрать устойчивый локатор;
- проверить состояние страницы или элемента;
- объяснить, почему `waitForTimeout` обычно не нужен.


## Цель модуля

Цель модуля — научиться находить элементы на странице, выполнять с ними действия и писать надежные проверки.

После модуля вы сможете:

- объяснить, что такое locator;
- выбирать стабильные локаторы;
- использовать `getByRole`, `getByText`, `getByLabel`, `getByPlaceholder` и `getByTestId`;
- применять простые CSS-локаторы и объяснять, почему XPath не стоит выбирать первым;
- писать полезные assertions;
- объяснять работу auto-waiting в Playwright;
- избегать фиксированных ожиданий через `waitForTimeout`;
- находить причины простых проблем с локаторами.

## Что уже нужно знать

Перед началом пригодятся знания из модулей 03–07:

- основы TypeScript;
- `async`/`await` и Promise;
- структура теста Playwright;
- назначение `test`, `expect` и `page`;
- базовые действия `goto`, `click`, `fill` и `press`.

## Почему локаторы важны

Locator — это способ, которым тест находит элемент. Нестабильный locator приводит к нестабильным, или flaky, тестам. Хороший locator описывает элемент так, как его видит или использует человек. Поэтому выбор локаторов — один из важнейших навыков в автоматизации UI.

## Темы модуля

- понятие locator и его отличие от элемента;
- локаторы, ориентированные на пользователя;
- `getByRole`, `getByText`, `getByLabel`, `getByPlaceholder` и `getByTestId`;
- CSS-локаторы и обзор XPath;
- последовательное уточнение и фильтрация локаторов;
- действия и assertions;
- auto-waiting;
- типичные ошибки.

## 1. Что такое locator

Locator — не сам элемент, а способ найти его на странице.

```ts
const getStartedLink = page.getByRole('link', { name: 'Get started' });

await getStartedLink.click();
```

- В `getStartedLink` хранится locator.
- Playwright ищет фактический элемент при выполнении действия или assertion.
- Благодаря этому Playwright может правильно дождаться элемента.

## 2. Locator и element

В Playwright мы обычно работаем с локаторами, а не с необработанными element handles.

```ts
await page.getByRole('button', { name: 'Login' }).click();
```

Не следует мыслить по принципу «найти один раз и хранить всегда». Locator может заново проверить страницу в момент, когда это необходимо.

## 3. Рекомендуемый приоритет локаторов

1. Локаторы, отражающие восприятие пользователя:
    - `getByRole`;
    - `getByLabel`;
    - `getByPlaceholder`;
    - `getByText`;
    - `getByAltText`;
    - `getByTitle`.
2. Специальный тестовый locator:
    - `getByTestId`.
3. CSS-локатор — когда возможностей предыдущих вариантов недостаточно.
4. XPath — только при ясной причине.

Начинайте с локаторов, которые описывают понимание страницы пользователем. Не пишите длинный CSS или XPath, если доступны роль, label или test id.

## 4. `getByRole`

`getByRole` часто оказывается лучшим вариантом: он использует доступную роль и имя элемента.

```ts
await page.getByRole('link', { name: 'Get started' }).click();
```

```ts
await page.getByRole('button', { name: 'Submit' }).click();
```

```ts
await expect(page.getByRole('heading', { name: 'Installation' })).toBeVisible();
```

Такой locator подходит для кнопок, ссылок, заголовков, флажков и других доступных элементов. Тест при этом остается близким к поведению пользователя.

## 5. `getByText`

`getByText` находит элемент по видимому тексту.

```ts
await expect(page.getByText('Installation')).toBeVisible();
```

Не выбирайте слишком общий текст, который встречается в нескольких элементах. Например, следующий locator может оказаться неоднозначным, если на странице несколько надписей «Save»:

```ts
await page.getByText('Save').click();
```

## 6. `getByLabel`

`getByLabel` удобен для полей формы, у которых есть label.

```ts
await page.getByLabel('Email').fill('qa.user@example.com');
await page.getByLabel('Password').fill('Password123!');
```

Если у поля есть label, такой вариант обычно лучше выбора `input` через CSS.

## 7. `getByPlaceholder`

`getByPlaceholder` удобен, когда у поля есть понятный placeholder.

```ts
await page.getByPlaceholder('What needs to be done?').fill('Buy milk');
```

Текст placeholder может измениться, поэтому оценивайте его стабильность.

## 8. `getByTestId`

`getByTestId` полезен, если приложение предоставляет стабильные тестовые атрибуты.

```ts
await page.getByTestId('login-button').click();
```

Test id помогает, когда видимый текст нестабилен. Названия должны быть осмысленными, а используемый атрибут команда обычно согласовывает с разработчиками. По умолчанию Playwright использует `data-testid`.

## 9. CSS-локаторы

CSS-локаторы обладают широкими возможностями, но не должны быть первым вариантом, если есть более подходящий locator Playwright.

```ts
await page.locator('.todo-list li').first().click();
```

```ts
await page.locator('[data-testid="login-button"]').click();
```

Длинные CSS-пути хрупки. Избегайте зависимостей от расположения элемента и сгенерированных имен классов.

## 10. XPath: обзорно

Playwright поддерживает XPath, но обычно его не стоит выбирать первым.

```ts
await page.locator('//button[text()="Login"]').click();
```

XPath иногда полезен в устаревших приложениях, однако длинные выражения трудно поддерживать. По возможности выбирайте роль, label, текст, test id или простой CSS. Подробное изучение XPath не входит в этот модуль.

## 11. Последовательное уточнение локаторов

Иногда сначала нужно найти область страницы, а затем элемент внутри нее.

```ts
const todoItem = page.getByText('Buy milk');

await expect(todoItem).toBeVisible();
```

Более структурированный вариант ограничивает поиск списком:

```ts
const todoList = page.locator('.todo-list');

await expect(todoList.getByText('Buy milk')).toBeVisible();
```

Это уменьшает область поиска, когда нужен элемент внутри определенного блока.

## 12. Фильтрация локаторов

Метод `filter` помогает выбрать нужный элемент среди похожих.

```ts
const item = page
  .locator('li')
  .filter({ hasText: 'Buy milk' });

await expect(item).toBeVisible();
```

На этом этапе достаточно уметь отфильтровать элементы по конкретному тексту.

## 13. Действия с локаторами

```ts
await page.getByRole('button', { name: 'Login' }).click();
```

```ts
await page.getByLabel('Email').fill('qa.user@example.com');
```

```ts
await page.getByPlaceholder('Search').press('Enter');
```

Асинхронные действия с локаторами обычно требуют `await`.

## 14. Assertions

Assertion проверяет ожидаемый результат. Тест без assertions обычно слаб: он выполняет действия, но не доказывает, что система повела себя правильно.

```ts
await expect(page).toHaveTitle(/Playwright/);
```

```ts
await expect(page).toHaveURL(/.*intro/);
```

```ts
await expect(page.getByText('Buy milk')).toBeVisible();
```

```ts
await expect(page.getByRole('button', { name: 'Submit' })).toBeEnabled();
```

```ts
await expect(page.getByRole('button', { name: 'Submit' })).toBeDisabled();
```

```ts
await expect(page.getByText('Error message')).toHaveText('Error message');
```

Проверяйте результат, важный для бизнес-сценария. Не ограничивайтесь проверкой, что «что-то существует», если цель теста заключается в другом.

## 15. Auto-waiting

Перед многими действиями Playwright автоматически ожидает, что элемент:

- присутствует в DOM;
- видим;
- стабилен;
- может получить действие.

Web-first assertions также ожидают выполнения условия в пределах timeout.

```ts
await page.getByRole('button', { name: 'Save' }).click();
await expect(page.getByText('Saved successfully')).toBeVisible();
```

В обычном случае фиксированное ожидание между этими шагами не требуется.

## 16. Почему не нужно злоупотреблять `waitForTimeout`

Фиксированные ожидания замедляют тесты и делают их нестабильными: короткого интервала иногда не хватит, а слишком длинный интервал всегда тратит время.

Плохой вариант:

```ts
await page.waitForTimeout(5000);
await expect(page.getByText('Saved successfully')).toBeVisible();
```

Лучше дождаться нужного состояния:

```ts
await expect(page.getByText('Saved successfully')).toBeVisible();
```

Assertion сам ожидает выполнения условия.

## 17. Отладка проблем с локаторами

1. Прочитайте сообщение об ошибке.
2. Проверьте текст и область поиска locator.
3. Убедитесь, что locator не соответствует нескольким элементам.
4. Запустите тест в headed mode.
5. Используйте UI mode или Playwright Inspector.
6. Если trace уже записан, откройте его в Trace Viewer.

```bash
npm run test:headed
npm run test:ui
npm run test:debug
npm run report
```

## 18. Типичные ошибки

- слишком общий текстовый locator;
- длинный и хрупкий CSS-путь;
- выбор XPath по умолчанию;
- пропущенный `await`;
- фиксированные ожидания;
- тесты без assertions;
- проверка деталей реализации вместо видимого результата;
- locator, случайно соответствующий нескольким элементам;
- сгенерированные имена классов;
- чрезмерное использование `nth()`.

## Что обязательно понять в этом модуле

- что такое locator;
- как применять `getByRole`, `getByText`, `getByLabel`, `getByPlaceholder` и `getByTestId`;
- как писать базовые assertions;
- как работает auto-waiting;
- почему фиксированные ожидания вредны;
- как разобрать простую проблему с locator.

## Что пока достаточно узнать обзорно

- CSS-локаторы;
- XPath;
- фильтрация и последовательное уточнение локаторов;
- применение Trace Viewer для отладки locator.

## Практика на занятии

Практика выполняется в отдельном репозитории домашних заданий:

1. Откройте репозиторий в VS Code.
2. Проверьте текущую ветку.
3. Перейдите в личную ветку `master` и получите обновления.
4. Перейдите в ветку модуля 08.
5. До начала работы добавьте в нее изменения из личной ветки `master`.
6. Создайте тесты Playwright для тренировки локаторов.
7. Примените `getByRole`.
8. Примените `getByText`.
9. Примените `getByLabel` или `getByPlaceholder`.
10. Примените `getByTestId`, если выбранная страница его поддерживает.
11. Добавьте assertions.
12. Удалите фиксированные ожидания, если они появились.
13. Запустите тесты.
14. При ошибке откройте отчет.

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-08-locators-assertions
git merge origin/student/{student-name-slug}/master
git push
```

```bash
npm install
npx playwright install
npm run test
npm run test:headed
npm run report
```

Команда `npx playwright install` нужна только в том случае, если браузеры еще не установлены локально.

## Вопросы для проверки понимания

1. Что такое locator?
2. Почему locator — не то же самое, что найденный элемент?
3. Почему `getByRole` часто является хорошим выбором?
4. Когда полезен `getByText`?
5. В каких случаях `getByText` может быть нестабильным?
6. Для чего применяется `getByLabel`?
7. Для чего применяется `getByPlaceholder`?
8. Когда нужен `getByTestId` и какой атрибут он использует по умолчанию?
9. Чем опасны длинные CSS-локаторы?
10. Почему XPath обычно не должен быть первым выбором?
11. Что означает последовательное уточнение локаторов?
12. Для чего нужна фильтрация локаторов?
13. Что такое assertion?
14. Почему тесту нужны assertions?
15. Что означает auto-waiting в Playwright?
16. Почему `waitForTimeout` обычно ухудшает тест?
17. Как UI mode помогает отлаживать locator?
18. Какие распространенные ошибки делают при выборе локаторов?

## Краткий итог

Теперь вы понимаете назначение локаторов, можете выбирать более стабильные варианты и писать полезные assertions. Вы знаете, как auto-waiting заменяет большинство фиксированных ожиданий, и умеете начать отладку простой проблемы с locator. Эти навыки понадобятся позднее, когда локаторы будут вынесены в Page Objects; реализация Page Object Model относится к следующим модулям.

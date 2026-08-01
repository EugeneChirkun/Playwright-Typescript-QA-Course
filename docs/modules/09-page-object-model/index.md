# Модуль 09: Page Object Model и clean code

## Цель модуля

Цель модуля — научиться организовывать UI-тесты Playwright так, чтобы тестовые файлы оставались читаемыми, а детали конкретных страниц находились в отдельных classes.

После модуля вы сможете:

- объяснить, что такое Page Object Model и зачем он нужен;
- создать простой Page Object class;
- передать Playwright `page` в class через constructor;
- хранить locators внутри Page Object;
- создавать methods для действий пользователя;
- использовать Page Object в тесте;
- не усложнять Page Objects без необходимости;
- сохранять тесты читаемыми;
- отделять сценарий теста от деталей реализации страницы.

## Что уже нужно знать

Перед началом пригодятся основы TypeScript, `async`/`await`, classes и constructors из модулей 03–06. Также нужно понимать Playwright `test`, `expect` и `page`, locators, assertions из модулей 07–08 и базовый порядок работы с Git.

## Почему Page Object Model нужен

Без Page Object Model тесты часто становятся сложными в сопровождении: каждый из них содержит адреса страниц, locators, клики, заполнение полей, assertions и повторяющиеся шаги.

Page Object Model помогает:

- уменьшить дублирование;
- сохранить тесты читаемыми;
- держать locators в одном месте;
- проще вносить изменения при обновлении интерфейса;
- отделить намерение теста от деталей страницы.

## Темы модуля

- проблема дублирования кода UI-тестов;
- назначение Page Object Model;
- Page Object class;
- constructor с `Page`;
- свойства с locators;
- methods действий и assertions;
- читаемость теста и выбор названий;
- содержимое Page Object и его границы;
- типичные ошибки;
- базовая структура папок.

## 1. Проблема: тесты становятся слишком подробными

```ts
import { expect, test } from '@playwright/test';

test('user can add todo item', async ({ page }) => {
  await page.goto('https://demo.playwright.dev/todomvc/');

  await page.getByPlaceholder('What needs to be done?').fill('Buy milk');
  await page.getByPlaceholder('What needs to be done?').press('Enter');

  await expect(page.getByText('Buy milk')).toBeVisible();
});
```

Для первых тестов такой вариант приемлем. Но если множество тестов используют одну страницу и одни locators, возникает дублирование. После изменения интерфейса приходится исправлять сразу много тестовых файлов.

## 2. Что такое Page Object Model

Page Object Model — подход, при котором каждая важная страница или область страницы представлена отдельным class. В нем обычно находятся locators, действия со страницей и иногда относящиеся к ней assertions.

Тест описывает сценарий, а Page Object — способ взаимодействия со страницей.

## 3. Простой Page Object class

Создадим `TodoPage`:

```ts
import { expect, Page } from '@playwright/test';

export class TodoPage {
  constructor(private page: Page) {}

  async open(): Promise<void> {
    await this.page.goto('https://demo.playwright.dev/todomvc/');
  }

  async addTodo(title: string): Promise<void> {
    await this.page.getByPlaceholder('What needs to be done?').fill(title);
    await this.page.getByPlaceholder('What needs to be done?').press('Enter');
  }

  async expectTodoVisible(title: string): Promise<void> {
    await expect(this.page.getByText(title)).toBeVisible();
  }
}
```

`TodoPage` представляет страницу TodoMVC. Constructor получает Playwright `page`, а methods описывают действия на странице. Благодаря этому тест становится короче.

## 4. Использование Page Object в тесте

```ts
import { test } from '@playwright/test';
import { TodoPage } from '../../src/pages/TodoPage';

test('user can add todo item', async ({ page }) => {
  const todoPage = new TodoPage(page);

  await todoPage.open();
  await todoPage.addTodo('Buy milk');
  await todoPage.expectTodoVisible('Buy milk');
});
```

Такой тест проще читать: locators скрыты внутри `TodoPage`, а названия methods показывают шаги сценария.

## 5. Constructor и объект `Page`

Page Object нужен Playwright `Page`, чтобы взаимодействовать со страницей браузера:

```ts
import { Page } from '@playwright/test';

export class PlaywrightHomePage {
  constructor(private page: Page) {}
}
```

Запись `private page: Page` сохраняет объект страницы внутри class. Затем methods обращаются к нему через `this.page`. Так знания ООП из модуля 06 применяются вместе с Playwright.

## 6. Свойства с locators

Locators можно хранить в свойствах class или возвращать из methods.

```ts
import { Locator, Page } from '@playwright/test';

export class PlaywrightHomePage {
  readonly getStartedLink: Locator;

  constructor(private page: Page) {
    this.getStartedLink = page.getByRole('link', { name: 'Get started' });
  }

  async open(): Promise<void> {
    await this.page.goto('https://playwright.dev/');
  }

  async clickGetStarted(): Promise<void> {
    await this.getStartedLink.click();
  }
}
```

Тип `Locator` импортируется из Playwright. Модификатор `readonly` полезен, потому что свойство с locator не должно получать новое значение. Название method должно описывать действие пользователя.

Необязательная альтернатива — закрытое вычисляемое свойство:

```ts
private get todoInput(): Locator {
  return this.page.getByPlaceholder('What needs to be done?');
}
```

На начальном этапе выберите один понятный вариант и применяйте его последовательно.

## 7. Methods действий

Methods действий выполняют шаги пользователя: открывают страницу, нажимают ссылку, заполняют поле, отправляют форму или добавляют задачу.

Понятные названия: `open()`, `addTodo(title: string)`, `clickGetStarted()`, `searchFor(text: string)`. Названия `clickButton1()`, `doStuff()`, `testAction()` и просто `click()` не объясняют цель действия.

## 8. Methods с assertions

Распространены два подхода:

1. Оставлять assertions в тестовых файлах.
2. Помещать относящиеся к странице assertions в methods Page Object.

Оба подхода допустимы, если команда следует выбранному правилу последовательно.

Assertion внутри Page Object:

```ts
async expectTodoVisible(title: string): Promise<void> {
  await expect(this.page.getByText(title)).toBeVisible();
}
```

Assertion в тестовом файле:

```ts
await expect(page.getByText('Buy milk')).toBeVisible();
```

В учебном проекте page-specific methods с assertions допустимы. При этом важные бизнес-проверки не следует прятать слишком глубоко: смысл теста должен оставаться понятным.

## 9. Чистый тест и скрытая логика

Page Object должен делать тест яснее, а не загадочнее.

Хороший пример:

```ts
test('user can add todo item', async ({ page }) => {
  const todoPage = new TodoPage(page);

  await todoPage.open();
  await todoPage.addTodo('Buy milk');
  await todoPage.expectTodoVisible('Buy milk');
});
```

Плохой пример:

```ts
test('todo test', async ({ page }) => {
  const todoPage = new TodoPage(page);

  await todoPage.doEverything();
});
```

`doEverything` скрывает весь сценарий. Хорошие названия methods позволяют прочитать последовательность действий без изучения реализации Page Object.

## 10. Структура папок

Для репозитория с домашними заданиями достаточно простой структуры:

```text
src/
  pages/
    TodoPage.ts
    PlaywrightHomePage.ts

tests/
  module-09-page-object-model/
    todo-page-object.spec.ts
    playwright-home-page-object.spec.ts
```

В `src/pages/` находятся Page Object classes, а в `tests/` — тесты, которые их импортируют.

## 11. Что должно находиться внутри Page Object

Подходящее содержимое:

- адрес страницы;
- locators;
- действия, относящиеся к странице;
- assertions, относящиеся к странице;
- небольшие вспомогательные methods для этой страницы.

Не следует помещать туда:

- логику test runner;
- несвязанные тестовые данные;
- API clients;
- бизнес-решения, относящиеся к сценарию теста;
- огромный «god class» для всего приложения;
- methods, которые скрывают весь тестовый сценарий.

## 12. Выбор названий

Название class должно описывать страницу или компонент, название method — действие пользователя или ожидаемое состояние, а имя locator — конкретный элемент.

Хорошие примеры: `TodoPage`, `PlaywrightHomePage`, `open()`, `addTodo(title)`, `expectTodoVisible(title)`, `getStartedLink`.

Избегайте названий `Page1`, `CommonPageForEverything`, `click1`, `doAction`, `check()`.

## 13. Page Object и компоненты

Повторяющиеся части интерфейса иногда представляют отдельными объектами-компонентами: шапку сайта, меню навигации, модальное окно, таблицу или боковую панель. Пока достаточно знать о такой возможности; сложную модель компонентов в этом модуле мы не реализуем.

## 14. Типичные ошибки

- помещать все страницы приложения в один огромный class;
- создавать отдельный method для каждой мелкой команды Playwright без пользы;
- скрывать весь тест в одном method;
- использовать неясные названия;
- смешивать несвязанные страницы в одном class;
- без причины хранить тестовые данные внутри Page Object;
- забывать `await`;
- дублировать locators в тестах и Page Objects;
- злоупотреблять наследованием;
- создавать Page Object до того, как стало понятно поведение страницы.

## Что обязательно понять в этом модуле

- что такое Page Object Model и почему он уменьшает дублирование;
- как создать Page Object class и передать `page` через constructor;
- как хранить locators и создавать methods действий;
- как использовать Page Object в тестах;
- как сохранить тесты читаемыми.

## Что пока достаточно узнать обзорно

Пока достаточно общего представления об объектах-компонентах, Page Objects на основе fixtures и более сложной архитектуре. Fixtures и настройка авторизации будут рассмотрены в модуле 10; здесь мы их не реализуем.

## Практика на занятии

1. Откройте репозиторий с домашними заданиями в VS Code.
2. Проверьте текущую ветку.
3. Переключитесь на личную master-ветку и получите обновления.
4. Переключитесь на ветку модуля 09.
5. Перед началом работы добавьте в нее изменения из личной master-ветки.
6. Создайте Page Object class для TodoMVC.
7. Перенесите в него locators и действия.
8. Создайте тест, использующий Page Object.
9. Создайте Page Object class для главной страницы Playwright.
10. Создайте использующий его тест.
11. Запустите тесты.
12. При необходимости откройте отчет.

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-09-page-object-model
git merge origin/student/{student-name-slug}/master
git push
```

```bash
npm install
npx playwright install
npm run test
npm run report
```

Команда `npx playwright install` нужна только в том случае, если браузеры еще не установлены локально.

## Вопросы для проверки понимания

1. Какую проблему решает Page Object Model?
2. Что такое Page Object?
3. Почему не стоит дублировать locators во многих тестах?
4. Что обычно хранится в Page Object?
5. Что не следует хранить в Page Object?
6. Зачем Page Object получает `page` через constructor?
7. Что означает `this.page`?
8. Что такое method действия?
9. Что такое method с assertion?
10. Почему названия methods должны быть понятными?
11. Почему `doEverything` — плохое название?
12. Где следует хранить Page Object classes?
13. Где следует хранить тесты?
14. Когда assertion можно поместить в Page Object?
15. Когда assertion лучше оставить в тесте?
16. Что называют «god class»?
17. Как Page Object Model готовит проект к росту набора автотестов?
18. Как fixtures позднее помогут создавать и переиспользовать Page Objects?

## Краткий итог

Вы изучили основы Page Object Model, научились создавать Page Object classes и выносить из тестовых файлов locators и действия со страницей. Теперь вы можете писать более чистые тесты Playwright и готовы перейти в модуле 10 к fixtures, тестовым данным и настройке авторизации.

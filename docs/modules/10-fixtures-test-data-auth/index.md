# Модуль 10: Fixtures, test data и auth

## Цель модуля

Цель модуля — научиться готовить переиспользуемую настройку тестов в Playwright. Материал опирается на знания TypeScript, `async`/`await`, ООП, Playwright, локаторов, проверок и Page Object Model из модулей 03–09.

После модуля вы сможете:

- объяснить, что такое fixture;
- использовать встроенные fixtures Playwright;
- создать простой custom fixture и понять `test.extend`;
- организовать типизированные test data;
- не дублировать пользователей и учетные данные в каждом тесте;
- объяснить назначение `storageState` и пользу сохраненного состояния авторизации;
- подготовить простой auth setup;
- оставлять в сценарии теста саму проверку, а не повторяющуюся подготовку.

## Почему это важно для Automation QA

В реальных проектах тесты часто требуют одинакового пользователя, Page Object, набора данных или состояния авторизации. Дублирование усложняет чтение, ревью и изменение тестов. Fixtures позволяют вынести повторяющуюся подготовку в одно место, организованные test data делают намерение сценария понятнее, а auth setup избавляет от входа через UI в каждом тесте.

## Темы модуля

- встроенные fixtures Playwright: `page`, `context`, `browser`, `browserName`;
- назначение fixtures;
- custom fixtures и `test.extend`;
- Page Object как fixture;
- отдельные файлы и типизация test data;
- пользователи и роли;
- auth setup и `storageState`;
- setup project;
- типичные ошибки.

## 1. Что такое fixture

Fixture — это подготовленные данные, объект или настройка, которые тест получает перед выполнением. Playwright готовит ресурс, тест использует его, а после теста Playwright при необходимости выполняет очистку.

```ts
import { test, expect } from '@playwright/test';

test('homepage has title', async ({ page }) => {
  await page.goto('/');

  await expect(page).toHaveTitle(/Example/);
});
```

Тест не создает `page` вручную: Playwright передает страницу в тест как fixture.

## 2. Встроенные fixtures Playwright

Playwright уже предоставляет готовые fixtures:

- `page` — отдельная вкладка браузера для теста;
- `context` — изолированный контекст браузера;
- `browser` — экземпляр браузера;
- `browserName` — имя текущего браузера;
- `request` — контекст для HTTP-запросов; он понадобится при дальнейшем изучении API testing.

```ts
import { test } from '@playwright/test';

test('browser name example', async ({ page, browserName }) => {
  await page.goto('/');

  console.log(browserName);
});
```

Запрашивайте только те fixtures, которые действительно нужны сценарию.

## 3. Почему не всё нужно писать прямо в тесте

В следующем примере подготовка дублируется:

```ts
import { test, expect } from '@playwright/test';
import { LoginPage } from '../src/pages/login.page';

test('admin can open dashboard', async ({ page }) => {
  const loginPage = new LoginPage(page);

  await loginPage.open();
  await loginPage.login('admin@example.com', 'Password123!');

  await expect(page.getByText('Dashboard')).toBeVisible();
});

test('manager can open dashboard', async ({ page }) => {
  const loginPage = new LoginPage(page);

  await loginPage.open();
  await loginPage.login('manager@example.com', 'Password123!');

  await expect(page.getByText('Dashboard')).toBeVisible();
});
```

Здесь повторяются создание Page Object, шаги входа и пароль. Из-за лишней подготовки тесты становятся шумными, а изменение общего шага приходится вносить в нескольких местах.

## 4. Организация test data

Test data лучше хранить отдельно от логики теста.

```ts
// src/test-data/users.ts
export type UserRole = 'admin' | 'manager' | 'viewer';

export interface TestUser {
  email: string;
  password: string;
  role: UserRole;
}

export const adminUser: TestUser = {
  email: 'admin@example.com',
  password: 'Password123!',
  role: 'admin',
};

export const managerUser: TestUser = {
  email: 'manager@example.com',
  password: 'Password123!',
  role: 'manager',
};
```

Такие данные можно переиспользовать. Union type ограничивает допустимые роли, а тест импортирует пользователя вместо повторения значений. В учебных примерах используйте только демонстрационные учетные данные.

## 5. Custom fixture

Custom fixture — fixture, которую мы создаем для потребностей своего проекта.

```ts
// tests/fixtures/pages.fixture.ts
import { test as base } from '@playwright/test';
import { LoginPage } from '../../src/pages/login.page';

type Pages = {
  loginPage: LoginPage;
};

export const test = base.extend<Pages>({
  loginPage: async ({ page }, use) => {
    const loginPage = new LoginPage(page);

    await use(loginPage);
  },
});

export { expect } from '@playwright/test';
```

`base.extend` создает расширенную версию `test`. Новая fixture `loginPage` становится доступна тестам, а `use(loginPage)` передает подготовленный объект в выполняемый тест.

## 6. Тест с custom fixture

```ts
import { test, expect } from './fixtures/pages.fixture';

test('login page has login button', async ({ loginPage }) => {
  await loginPage.open();

  await loginPage.expectLoginButtonVisible();
});
```

Тест больше не создает `LoginPage` вручную. Подготовка находится в fixture, поэтому сценарий стал короче и яснее. Импортированный `expect` доступен, когда сценарию нужны дополнительные проверки.

## 7. Как работает `test.extend`

Последовательность действий:

1. Импортировать Playwright `test` под именем `base`.
2. Описать типы дополнительных fixtures.
3. Внутри каждой fixture создать нужный объект.
4. Передать объект тесту через `use`.
5. В spec-файлах импортировать custom `test`, а не исходный `test` Playwright.

Этого достаточно для первых fixtures. Подробный жизненный цикл и сложные области видимости пока не нужны.

## 8. Fixtures и Page Object Model

Fixtures часто применяют вместе с Page Object Model. Например, одна fixture предоставляет `LoginPage`, а другая — `DashboardPage`. Позже список можно расширить:

```ts
type Pages = {
  loginPage: LoginPage;
  dashboardPage: DashboardPage;
};
```

Создание Page Objects оказывается в одном месте, а тест остается сосредоточен на поведении пользователя. Не объединяйте все объекты проекта в одну огромную fixture без необходимости.

## 9. Auth в автотестах

Многим тестам нужен уже авторизованный пользователь. Повторный вход через UI в каждом сценарии замедляет выполнение и скрывает основную проверку.

Есть два базовых подхода:

- выполнить вход через UI непосредственно в тесте или fixture;
- один раз подготовить состояние авторизации и переиспользовать его.

Во втором подходе Playwright использует `storageState`.

## 10. Что такое `storageState`

`storageState` — сохраненное состояние браузера: cookies, local storage и связанные с авторизацией браузерные данные. Если приложение сохраняет результат входа в cookies или local storage, Playwright может записать это состояние в файл и загрузить его для следующих тестов.

```ts
await page.context().storageState({ path: 'playwright/.auth/user.json' });
```

Файл позволяет переиспользовать авторизованное состояние, но может содержать чувствительные данные, поэтому обращаться с ним нужно осторожно.

## 11. Файл auth setup

```ts
// tests/auth.setup.ts
import { test as setup } from '@playwright/test';
import { LoginPage } from '../src/pages/login.page';
import { adminUser } from '../src/test-data/users';

setup('authenticate as admin', async ({ page }) => {
  const loginPage = new LoginPage(page);

  await loginPage.open();
  await loginPage.login(adminUser.email, adminUser.password);

  await page.context().storageState({ path: 'playwright/.auth/admin.json' });
});
```

Этот setup выполняет вход один раз и сохраняет состояние. Затем его могут использовать обычные тесты. В реальном проекте после входа стоит дождаться признака успешной авторизации перед сохранением файла.

## 12. Использование `storageState` в конфигурации

```ts
// playwright.config.ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './tests',
  projects: [
    {
      name: 'setup',
      testMatch: /.*\.setup\.ts/,
    },
    {
      name: 'chromium',
      use: {
        ...devices['Desktop Chrome'],
        storageState: 'playwright/.auth/admin.json',
      },
      dependencies: ['setup'],
    },
  ],
});
```

Setup project запускается первым. Проект `chromium` зависит от него и начинает тесты с сохраненным состоянием авторизации. На этом этапе достаточно понимать эту связь без углубления в сложные зависимости проектов.

## 13. Безопасность

Файлы состояния авторизации могут содержать чувствительные данные. Не добавляйте в Git реальные секреты, токены, учетные данные production или созданные auth-файлы. Добавьте каталог в `.gitignore` учебного проекта:

```text
playwright/.auth/
```

Для обучения подходят демонстрационные данные. В реальных проектах учетные данные и auth-файлы требуют дополнительных правил хранения, которые рассматриваются отдельно.

## 14. Совместное использование test data и auth

Auth setup может получать пользователя из единого файла данных:

```ts
import { adminUser } from '../src/test-data/users';

await loginPage.login(adminUser.email, adminUser.password);
```

Учетные данные не разбросаны по тестам. Если демонстрационные данные изменятся, достаточно обновить одно место.

## 15. Типичные ошибки

- Создавать fixtures заранее для каждого значения, даже если повторения еще нет.
- Помещать проверки в fixtures вместо тестовых сценариев.
- Скрывать во fixture слишком много значимой логики теста.
- Дублировать test data в разных файлах.
- Добавлять в Git auth-состояние с реальными токенами.
- Забывать про `playwright/.auth/` в `.gitignore`.
- Использовать одного пользователя во всех тестах, когда нужны разные роли.
- Создавать один огромный файл fixtures.
- Импортировать исходный Playwright `test`, когда сценарию нужен custom `test`.

## Что обязательно понять в этом модуле

- Что такое fixture и почему `page` — встроенная fixture.
- Как создать custom fixture через `test.extend` и передать значение через `use`.
- Зачем хранить test data в отдельном файле.
- Что сохраняет `storageState` и какова идея auth setup.
- Почему чувствительные auth-файлы нельзя добавлять в Git.

## Что пока достаточно узнать обзорно

- Setup project в конфигурации Playwright.
- Зависимости между проектами.
- Работа с несколькими авторизованными пользователями.
- Fixture `request` для будущего изучения API testing.

## Практика на занятии

Практика выполняется в отдельном репозитории домашних заданий `Pw-Ts-Qa-Hw`.

1. Откройте репозиторий домашних заданий в VS Code.
2. Проверьте текущую ветку.
3. Переключитесь на личную ветку `master` и получите обновления.
4. Переключитесь на ветку модуля 10.
5. Перед работой добавьте в нее изменения из личной ветки `master`.
6. Создайте файл test data.
7. Создайте custom fixture для Page Objects.
8. Напишите один тест с custom fixture.
9. Добавьте простой пример auth setup.
10. Добавьте auth-каталог в `.gitignore`.
11. Запустите typecheck.
12. Запустите тесты, если учебное приложение поддерживает нужный сценарий.

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-10-fixtures-test-data-auth
git merge origin/student/{student-name-slug}/master
git push
```

```bash
npm run typecheck
npm run test
```

## Вопросы для проверки понимания

1. Что такое fixture?
2. Какие встроенные fixtures предоставляет Playwright?
3. Почему `page` считается fixture?
4. Что такое custom fixture?
5. Что делает `test.extend`?
6. Для чего внутри fixture вызывается `use`?
7. Почему повторяющуюся подготовку полезно выносить из тестов?
8. Почему test data следует хранить отдельно от тестовой логики?
9. Что такое типизированные test data?
10. Зачем ограничивать роли пользователей через union type?
11. Что такое `storageState`?
12. Какие браузерные данные могут попасть в `storageState`?
13. Почему auth setup может ускорить набор тестов?
14. Почему реальные auth-файлы нельзя добавлять в Git?
15. Какую запись нужно добавить в `.gitignore`?
16. Как fixtures связаны с Page Object Model?
17. Каких ошибок следует избегать при создании fixtures?
18. Когда подготовку лучше оставить внутри теста, а не выносить во fixture?

## Краткий итог

Вы познакомились со встроенными fixtures Playwright, умеете создать простой custom fixture и организовать типизированные test data. Вы понимаете идею auth setup и `storageState`, а также готовы уменьшать дублирование в более крупных Playwright-проектах.

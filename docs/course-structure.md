# Структура курса

| Модуль | Тема | Основной фокус | Ожидаемый результат |
| --- | --- | --- | --- |
| [01](modules/01-environment-node-npm-vscode-playwright/index.md) | Окружение и Playwright | Node.js, npm, VS Code и базовая настройка | Студент проверяет окружение и запускает подготовленный проект Playwright. |
| [02](modules/02-git-branches-main-commands/index.md) | Git и ветки | Ветки, коммиты и рабочий процесс сдачи заданий | Студент обновляет ветку модуля, отправляет изменения и открывает PR. |
| [03](modules/03-typescript-introduction/index.md) | JavaScript и TypeScript: основы | Базовый синтаксис и первое знакомство с типами | Студент пишет простой и понятный типизированный код. |
| [04](modules/04-typescript-for-qa/index.md) | TypeScript для QA | Типы, test data, функции, `import` и `export` | Студент описывает QA-данные типами и разделяет код по файлам. |
| [05](modules/05-async-await-promises/index.md) | Async/await и Promise | Последовательный и параллельный асинхронный код | Студент объясняет назначение `Promise` и правильно использует `await`. |
| [06](modules/06-oop-basics/index.md) | ООП: основы | Классы, объекты, инкапсуляция и композиция | Студент создаёт небольшие классы и готов к изучению Page Object Model. |
| [07](modules/07-playwright-basics/index.md) | Playwright: основы | Структура, запуск и анализ первых UI-тестов | Студент пишет и запускает базовые тесты Playwright. |
| [08](modules/08-locators-assertions/index.md) | Локаторы и проверки | Устойчивые локаторы, проверки и автоматическое ожидание | Студент выбирает надёжные локаторы и пишет информативные проверки. |
| [09](modules/09-page-object-model/index.md) | Page Object Model | Структура и поддерживаемость UI-тестов | Студент выносит локаторы и действия в Page Objects без лишней логики. |
| [10](modules/10-fixtures-test-data-auth/index.md) | Fixtures, test data и auth | Переиспользование настройки, данных и состояния авторизации | Студент применяет custom fixtures и понимает подготовку `storageState`. |
| [11](modules/11-api-testing/index.md) | API testing | HTTP-запросы и проверки ответов через Playwright | Студент пишет безопасные учебные API-тесты с типизированными данными. |
| [12](modules/12-ci-final-project/index.md) | CI и финальный проект | GitHub Actions, итоговые UI- и API-тесты | Студент запускает проверки в CI и готовит финальный PR к ревью. |

После модуля 12 обязательная программа завершена: итоговые тесты выполняются локально и в CI, а результаты собраны в финальном PR.

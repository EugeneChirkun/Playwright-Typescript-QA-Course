# Модуль 12: CI и финальный проект

## Главное в модуле

- запуск проверок в GitHub Actions;
- диагностика падений и сохранение артефактов;
- сборка итоговых UI- и API-примеров.

## Минимальный результат

После модуля студент должен уметь:

- запустить typecheck и тесты в CI;
- найти причину неуспешного шага;
- подготовить финальный PR с результатами проверок.


## Цель модуля

Цель финального модуля — подключить проект на Playwright и TypeScript к CI и подготовить завершенный проект автоматизации, который удобно проверять на ревью.

После модуля вы сможете:

- объяснить, что такое CI и почему автоматизированные тесты должны запускаться в CI;
- создать workflow GitHub Actions для тестов Playwright;
- выполнить `npm ci` и `npm run typecheck` в CI;
- установить браузеры Playwright и запустить тесты в CI;
- загрузить HTML report как artifact;
- разобраться в основных причинах падения CI;
- подготовить структуру финального проекта и открыть итоговый PR на ревью.

## Почему CI важно для Automation QA

Автоматизированные тесты не должны запускаться только на локальном компьютере автора. CI выполняет одинаковые проверки в чистом окружении после отправки изменений и до их слияния. Команда получает общий результат, а ревьюеру проще понять, готов ли PR к проверке и merge.

Так раньше обнаруживаются сломанные тесты, ошибки типов и проблемы настройки. Успешный CI не заменяет code review, но дает воспроизводимую исходную точку для него.

## Темы модуля

- CI;
- GitHub Actions;
- файл workflow и trigger;
- job, runner и steps;
- `npm ci` и typecheck;
- установка браузеров и запуск тестов Playwright;
- HTML report как artifact;
- чтение журналов CI;
- структура финального проекта;
- итоговый PR и чеклист ревью.

## 1. Что такое CI

CI расшифровывается как Continuous Integration — непрерывная интеграция. На практике после push или создания pull request GitHub может автоматически:

1. установить зависимости;
2. проверить TypeScript;
3. установить браузеры;
4. запустить тесты;
5. сохранить отчет;
6. показать, можно ли безопасно переходить к ревью и merge PR.

## 2. GitHub Actions

GitHub Actions — встроенный в GitHub инструмент CI. Его workflow хранятся в каталоге:

```text
.github/workflows/
```

Workflow обычно описывается в YAML-файле. Он задает события запуска, окружение и последовательность команд.

## 3. Базовая структура workflow

Основные части workflow:

- `name` — понятное название;
- `on` — события, или triggers, для запуска;
- `jobs` — набор выполняемых задач;
- `runs-on` — runner, на котором выполняется job;
- `steps` — последовательные шаги job.

```yaml
name: Playwright Tests

on:
  push:
    branches:
      - master
      - 'student/**'
  pull_request:
    branches:
      - master
      - 'student/**'

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
```

`push` запускает workflow после push, а `pull_request` — для PR с указанной целевой веткой. `ubuntu-latest` выбирает Linux runner. Действие `actions/checkout` загружает код репозитория на runner.

## 4. npm ci

В CI следует предпочитать `npm ci`, а не `npm install`. Команда использует `package-lock.json`, требует его соответствия `package.json` и дает более предсказуемую установку зависимостей.

```yaml
- name: Install dependencies
  run: npm ci
```

## 5. Typecheck в CI

TypeScript следует проверить до запуска тестов:

```yaml
- name: Run TypeScript typecheck
  run: npm run typecheck
```

Если typecheck не прошел, проект уже содержит ошибку типов. Переходить к тестам и доверять их результату в таком состоянии не следует.

## 6. Установка браузеров Playwright в CI

На чистом CI runner браузеры Playwright могут отсутствовать, поэтому их нужно установить:

```yaml
- name: Install Playwright browsers
  run: npx playwright install --with-deps
```

Параметр `--with-deps` дополнительно устанавливает системные зависимости Linux, необходимые браузерам.

## 7. Запуск тестов Playwright в CI

```yaml
- name: Run Playwright tests
  run: npm run test
```

Команда должна совпадать со script в `package.json`. Если в проекте принято другое имя, workflow нужно согласовать с существующими scripts, а не создавать дублирующую команду.

## 8. Загрузка HTML report

При падении тестов HTML report помогает изучить шаги и ошибки. Его можно сохранить как artifact:

```yaml
- name: Upload Playwright report
  if: always()
  uses: actions/upload-artifact@v4
  with:
    name: playwright-report
    path: playwright-report/
    retention-days: 7
```

- `if: always()` запускает загрузку, даже если предыдущий step завершился ошибкой;
- отчет можно скачать из artifacts запуска GitHub Actions;
- `retention-days` определяет срок хранения artifact.

## 9. Полный workflow GitHub Actions

```yaml
name: Playwright Tests

on:
  push:
    branches:
      - master
      - 'student/**'
  pull_request:
    branches:
      - master
      - 'student/**'

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Run TypeScript typecheck
        run: npm run typecheck

      - name: Install Playwright browsers
        run: npx playwright install --with-deps

      - name: Run Playwright tests
        run: npm run test

      - name: Upload Playwright report
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: playwright-report
          path: playwright-report/
          retention-days: 7
```

Это стартовый workflow для репозитория домашних заданий. Позже его можно улучшить с учетом реальных потребностей проекта, но для финальной работы достаточно понятной последовательности проверок.

## 10. Чтение ошибок CI

Если CI завершился ошибкой, сначала найдите упавший step и прочитайте вывод его команды. Затем проверьте сообщения TypeScript или Playwright и доступные screenshot, trace и HTML report. Исправляйте первичную ошибку сверху по журналу, а после push убедитесь, что новый запуск начался.

Частые причины падения:

- зависимости не установлены;
- в `package.json` указано другое имя script;
- браузеры Playwright не установлены;
- тест проходит локально, но падает в CI из-за неверной работы со временем и ожиданиями;
- тест зависит от данных, доступных только локально;
- сгенерированные файлы ошибочно добавлены в Git или нужный файл отсутствует;
- для PR выбрана неверная целевая ветка.

## 11. Цель финального проекта

Финальный проект показывает, что вы умеете объединить темы курса в небольшом проекте на Playwright и TypeScript: от окружения, Git, типов, асинхронности и ООП до UI-тестов, API-тестов и CI.

В проекте должны быть:

- понятная структура каталогов;
- Page Object Model;
- стабильные locators и ясные assertions;
- типизированные тестовые данные;
- вспомогательные функции и fixtures;
- хотя бы один API-тест;
- workflow CI;
- README.md или итоговое описание результата.

## 12. Рекомендуемая структура финального проекта

```text
.github/
  workflows/
    playwright.yml

src/
  api/
    posts.api.ts
  pages/
    login.page.ts
    home.page.ts
  test-data/
    users.ts
    posts.ts
  helpers/
    string.helpers.ts

tests/
  fixtures/
    pages.fixture.ts
  module-12/
    final-ui.spec.ts
    final-api.spec.ts

homework/
  module-12-ci-final-project/
    result.md
```

Если похожие файлы уже появились в предыдущих модулях, адаптируйте структуру и переиспользуйте их. Не переносите код только ради полного совпадения с примером.

## 13. Требования к итоговому UI-тесту

Хотя бы один UI-тест должен:

- открыть страницу;
- использовать Page Object;
- выбрать стабильный locator;
- выполнить действие или проверку;
- содержать ясный assertion;
- по возможности не дублировать жестко заданные тестовые данные.

Реальная авторизация не обязательна, если подходящего демонстрационного приложения нет. Можно выбрать простой публичный сценарий из предыдущих модулей.

## 14. Требования к итоговому API-тесту

Хотя бы один API-тест должен:

- использовать fixture `request` из Playwright;
- отправить GET- или POST-запрос;
- проверить status code;
- проверить response body;
- использовать типизированные данные ответа или вспомогательную функцию.

Для учебного API-теста допустимо использовать `https://jsonplaceholder.typicode.com`.

## 15. Требования к fixtures и тестовым данным

Проект должен использовать:

- хотя бы один файл с тестовыми данными;
- хотя бы один типизированный объект или interface;
- хотя бы одну custom fixture, если уже существует код Page Object;
- переиспользуемую вспомогательную функцию или API helper.

## 16. Ожидания от итогового PR

Итоговый PR должен быть удобен для ревью. Добавьте осмысленное название и заполненное описание, укажите выполненные команды и результат CI. Перечислите реализованное, а также то, что не удалось реализовать, с объяснением причины. Если нужны решения ревьюера, сформулируйте конкретные вопросы.

## 17. Типичные ошибки

- создать workflow, но ни разу не проверить его запуск;
- забыть `npm ci` или установку браузеров Playwright;
- не загрузить HTML report;
- жестко записать учетные данные в коде;
- добавить в Git `node_modules`, состояние авторизации или отчеты;
- использовать нестабильные locators;
- скрыть слишком много логики во вспомогательных функциях;
- создать Page Object без реальной необходимости;
- открыть итоговый PR без описания;
- игнорировать падающий CI.

## Что обязательно понять в этом модуле

- что такое CI и workflow GitHub Actions;
- как устроен базовый workflow;
- зачем нужны `npm ci` и typecheck;
- как установить браузеры и запустить Playwright в CI;
- как сохранить HTML report как artifact;
- что ожидается от финального проекта.

## Что пока достаточно узнать обзорно

- secrets и переменные окружения;
- matrix strategy;
- углубленная оптимизация CI;
- параллельный запуск и sharding.

Для финального проекта не требуются Docker, Kubernetes, self-hosted runners, сложные стратегии и корпоративная архитектура CI. Secrets понадобятся позже для безопасного хранения чувствительных значений; реальные учетные данные нельзя добавлять в Git или учебный пример.

## Практика на занятии

1. Откройте репозиторий домашних заданий в VS Code.
2. Проверьте текущую ветку.
3. Перейдите в личную ветку `master` и получите обновления.
4. Перейдите в ветку модуля 12.
5. До начала работы выполните merge личной ветки `master` в ветку модуля.
6. Создайте workflow GitHub Actions.
7. Проверьте scripts в `package.json`.
8. Локально запустите typecheck.
9. Локально запустите тесты.
10. Отправьте изменения на GitHub.
11. Откройте PR.
12. Проверьте результат GitHub Actions.
13. Если artifact доступен, скачайте или откройте HTML report Playwright.
14. Заполните итоговое описание результата.

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-12-ci-final-project
git merge origin/student/{student-name-slug}/master
git push
```

Локальные проверки:

```bash
npm ci
npm run typecheck
npm run test
```

## Вопросы для проверки понимания

1. Что такое CI?
2. Почему автоматизированные тесты нужно запускать в CI?
3. Что такое GitHub Actions?
4. Где хранятся файлы workflow?
5. Что такое trigger workflow?
6. Что представляет собой job?
7. Для чего нужен runner?
8. Что выполняет step?
9. Почему `npm ci` полезнее для CI, чем `npm install`?
10. Почему typecheck следует запускать до тестов?
11. Почему браузеры Playwright нужно устанавливать на CI runner?
12. Что делает `npx playwright install --with-deps`?
13. Зачем загружать HTML report как artifact?
14. Что нужно проверить в первую очередь при падении CI?
15. Что должен включать финальный проект?
16. Почему итоговому PR нужно ясное описание?
17. Какие файлы нельзя добавлять в Git?
18. Почему нельзя использовать реальные учетные данные в тестах?
19. Как CI помогает при code review?

## Краткий итог

Вы познакомились с основами CI, умеете создать workflow GitHub Actions для Playwright, запустить typecheck и тесты и сохранить HTML report. Теперь вы можете собрать результаты всех модулей в финальный проект и подготовить его к ревью. После итогового PR и ревью курс завершен.

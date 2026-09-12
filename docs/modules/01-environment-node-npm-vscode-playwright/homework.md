# Домашнее задание: Модуль 01

## Цель домашнего задания

Проверить рабочее окружение, запустить подготовленный проект Playwright и зафиксировать результаты команд.

## Демонстрация и домашнее задание

На занятии может быть показана демонстрация создания нового проекта Playwright через `npm init playwright@latest`.

Домашнее задание выполняется в подготовленном репозитории домашних заданий:

`https://github.com/EugeneChirkun/Pw-Ts-Qa-Hw`

Для выполнения задания студент клонирует репозиторий, открывает его в VS Code, переходит в свою ветку модуля и заполняет файл `homework/module-01-environment/result.md`.

## Что нужно сдать

Основной результат модуля 01 — заполненный файл:

```text
homework/module-01-environment/result.md
```

В этом файле должны быть:

- версии Node.js, npm и Git;
- команды, которые были выполнены;
- результат установки зависимостей;
- результат `npm run typecheck`;
- результат `npm run test:smoke` или `npm run test`;
- краткое объяснение назначения `package.json`, `package-lock.json`, `node_modules` и `playwright.config.ts`;
- вопросы или проблемы, если они возникли.

Одних новых JSON-файлов или конфигурационных файлов недостаточно для сдачи модуля.

## Перед началом работы

Если репозиторий ещё не клонирован, выполните:

```bash
git clone https://github.com/EugeneChirkun/Pw-Ts-Qa-Hw.git
cd Pw-Ts-Qa-Hw
git fetch origin --prune
git switch --track origin/student/{student-name-slug}/module-01-environment
```

Если репозиторий и локальная ветка уже существуют:

```bash
git fetch origin --prune
git switch student/{student-name-slug}/module-01-environment
```

Если ветка не найдена, обратитесь к преподавателю. Не создавайте случайное имя ветки: используйте подготовленную ветку модуля.

## Задание 1. Проверить инструменты

Установите LTS-версию Node.js и VS Code, если они ещё не установлены. В терминале выполните:

```bash
node -v
npm -v
git --version
```

Сохраните версии для `result.md` и проверьте текущую ветку командой `git branch --show-current`.

## Задание 2. Изучить подготовленный проект

Откройте репозиторий в VS Code. Найдите `package.json`, `package-lock.json`, `playwright.config.ts`, `tests/` и `homework/`. Кратко опишите назначение основных файлов в `result.md`.

## Задание 3. Установить зависимости и браузеры

В корне репозитория домашних заданий выполните:

```bash
npm ci
npx playwright install
```

Команда `npx playwright install` нужна, если браузеры Playwright ещё не установлены локально. Если для вашего окружения потребовалась другая предусмотренная проектом команда установки, укажите её и результат в `result.md`.

## Задание 4. Запустить проверки

```bash
npm run typecheck
npm run test:smoke
npm run test
npm run report
```

Команда `npm run report` открывает HTML report после запуска тестов. Запишите результаты проверок и возникшие вопросы в `result.md`.

Не добавляйте в Git `node_modules`, `playwright-report` и `test-results`.

## Задание 5. Отправить результат

Проверьте и добавьте обязательный файл результата:

```bash
git status
git add homework/module-01-environment/result.md
git commit -m "Complete module 01 environment homework"
git push
```

Если преподаватель запросил снимки экрана или вы добавили их для пояснения результата, используйте:

```bash
git add homework/module-01-environment/result.md homework/module-01-environment/screenshots/
```

Откройте pull request (PR):

```text
base: student/{student-name-slug}/master
compare: student/{student-name-slug}/module-01-environment
```

## Ожидаемый результат

- [ ] Подготовленный репозиторий домашних заданий склонирован.
- [ ] Работа выполнена в правильной ветке `student/{student-name-slug}/module-01-environment`.
- [ ] Зависимости и браузеры Playwright установлены.
- [ ] Проверки типов, smoke-тест и остальные подготовленные тесты запущены.
- [ ] `homework/module-01-environment/result.md` заполнен результатами команд и пояснениями.
- [ ] PR открыт в личную ветку `student/{student-name-slug}/master`.
- [ ] `node_modules`, `playwright-report` и `test-results` не добавлены в Git.
- [ ] Результат включает заполненный `result.md`, а не только новые JSON-файлы или конфигурационные файлы.

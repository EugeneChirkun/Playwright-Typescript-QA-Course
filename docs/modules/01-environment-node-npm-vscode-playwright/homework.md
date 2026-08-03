# Домашнее задание: Модуль 01

## Цель домашнего задания

Проверить рабочее окружение и запустить уже подготовленный проект Playwright. Работа выполняется только в [репозитории домашних заданий](https://github.com/EugeneChirkun/Pw-Ts-Qa-Hw), а не в репозитории документации курса. Инициализировать ещё один проект Playwright не нужно.

## Задание 1. Проверить инструменты

Установите LTS-версию Node.js и VS Code, если они ещё не установлены. В терминале выполните:

```bash
node -v
npm -v
git --version
```

Сохраните версии для итогового описания.

## Задание 2. Подготовить ветку

Обновите ветку модуля из личной ветки `master` до редактирования файлов:

```bash
git clone https://github.com/EugeneChirkun/Pw-Ts-Qa-Hw
cd Pw-Ts-Qa-Hw
git fetch origin
git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master
git switch student/{student-name-slug}/module-01-environment
git merge origin/student/{student-name-slug}/master
```

Если ветка модуля ещё не создана, создайте её от обновлённой личной ветки:

```bash
git switch -c student/{student-name-slug}/module-01-environment
```

## Задание 3. Установить зависимости

В корне репозитория домашних заданий выполните:

```bash
npm ci
npx playwright install
```

Если `npm ci` сообщает, что `package-lock.json` отсутствует, используйте предусмотренную в репозитории команду установки и опишите это в результате. Не создавайте новый проект и не запускайте `npm init playwright@latest`.

## Задание 4. Проверить подготовленный проект

Найдите `package.json`, `playwright.config.ts` и каталог с тестами. В `homework/module-01-environment/result.md` запишите:

- версии Node.js, npm и Git;
- команды установки и запуска;
- назначение `package.json`, `package-lock.json` и `node_modules`;
- результат запуска тестов;
- возникшие вопросы или ограничения окружения.

Не добавляйте `node_modules`, `playwright-report` и `test-results` в Git.

## Задание 5. Запустить проверки

```bash
npm run typecheck
npm run test
```

Если в подготовленном репозитории пока нет тестов или скрипта запуска, зафиксируйте это в `result.md`; `npm run typecheck` должен проходить. При успешном запуске тестов откройте HTML report командой, предусмотренной в `package.json`.

## Задание 6. Отправить результат

```bash
git status
git add homework/module-01-environment/result.md
git commit -m "Complete module 01 environment homework"
git push -u origin student/{student-name-slug}/module-01-environment
```

Откройте pull request (PR):

```text
base: student/{student-name-slug}/master
compare: student/{student-name-slug}/module-01-environment
```

## Ожидаемый результат

- [ ] Работа выполнена в репозитории домашних заданий.
- [ ] Ветка модуля обновлена из личной ветки `master` до начала работы.
- [ ] Версии инструментов и результаты команд записаны в `result.md`.
- [ ] `npm run typecheck` проходит.
- [ ] Применимые тесты запущены, а ограничение окружения при необходимости описано.
- [ ] Сгенерированные файлы и `node_modules` не добавлены в Git.
- [ ] PR открыт в личную ветку `student/{student-name-slug}/master`.

# Домашнее задание: Модуль 02

## Цель домашнего задания

Цель — отработать реальный рабочий процесс, который используется в этом курсе:

- выполнить clone homework repository;
- создать личную основную ветку студента;
- создать ветку для Module 02;
- внести изменения;
- сделать commit;
- отправить изменения на GitHub;
- открыть pull request в личную основную ветку студента;
- обновить PR после ревью, если тьютор попросит внести исправления.

## Задание 1. Проверить Git

Выполните команду:

```bash
git --version
```

Если Git не установлен, установите его с официального сайта Git. Ссылка на страницу загрузки есть в ресурсах этого модуля.

## Задание 2. Настроить имя и email

Выполните команды:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global --list
```

Вместо `Your Name` и `your.email@example.com` укажите своё имя и email. Лучше использовать email, связанный с вашим GitHub-аккаунтом.

## Задание 3. Выполнить clone homework repository

Ссылку на homework repository предоставит тьютор. Это отдельный репозиторий для домашних заданий, а не репозиторий с документацией курса.

```bash
git clone <HOMEWORK_REPOSITORY_URL>
cd <PROJECT_FOLDER>
```

`<HOMEWORK_REPOSITORY_URL>` замените на реальную ссылку, которую даст тьютор. `<PROJECT_FOLDER>` замените на имя папки, которая появилась после clone.

## Задание 4. Создать личную основную ветку студента

Выполните команды:

```bash
git fetch origin
git switch -c student/<student-name>/master origin/master
git push -u origin student/<student-name>/master
```

Эта ветка — личная база студента для всех домашних заданий. Она создаётся один раз в начале работы с homework repository.

Студент не работает напрямую в общем `master` репозитория. Общий `master` должен оставаться чистой стартовой версией.

## Задание 5. Создать ветку для Module 02

Ветку задания нужно создать от личной основной ветки:

```bash
git switch student/<student-name>/master
git pull origin student/<student-name>/master
git switch -c student/<student-name>/module-02-git
```

Не создавайте ветку задания напрямую от общего `master`, если личная ветка `student/<student-name>/master` уже создана.

## Задание 6. Заполнить результат домашнего задания

Создайте или обновите файл:

```text
homework/module-02-git/result.md
```

Добавьте в файл:

- версию Git;
- имя ветки `student/<student-name>/module-02-git`;
- команды Git, которые вы использовали;
- короткое объяснение, что показывает `git status`;
- короткое объяснение, что делает `git add`;
- короткое объяснение, зачем нужен `git commit`;
- короткое объяснение, зачем нужен `git push`;
- ответ, почему `node_modules` не нужно добавлять в commit;
- вопросы или проблемы, если они появились.

## Задание 7. Сделать commit и отправить ветку

Выполните команды:

```bash
git status
git add .
git commit -m "Complete module 02 Git homework"
git push -u origin student/<student-name>/module-02-git
```

Перед commit внимательно посмотрите вывод `git status`, чтобы убедиться, что в изменения попали только нужные файлы.

## Задание 8. Открыть pull request

Откройте GitHub и перейдите в homework repository.

Создайте pull request с таким направлением:

- base: `student/<student-name>/master`
- compare: `student/<student-name>/module-02-git`

Добавьте короткое описание того, что было сделано, и отправьте PR на ревью тьютору.

Не открывайте PR в общий `master` homework repository.

## Задание 9. Внести исправления после ревью

Если тьютор оставил комментарии, внесите исправления в той же ветке `student/<student-name>/module-02-git`.

Затем выполните команды:

```bash
git status
git add .
git commit -m "Address review comments"
git push
```

После push pull request обновится автоматически.

## Ожидаемый результат

К концу задания должно быть готово:

- Git установлен;
- имя и email для Git настроены;
- homework repository склонирован на компьютер;
- ветка `student/<student-name>/master` создана;
- ветка `student/<student-name>/module-02-git` создана;
- файл `homework/module-02-git/result.md` заполнен;
- commit создан;
- ветка отправлена на GitHub;
- PR открыт в `student/<student-name>/master`;
- `node_modules` не добавлен в Git.

## Как подготовить результат для проверки

Отправьте тьютору:

- ссылку на PR;
- короткое описание того, что было сделано;
- список команд, которые вы использовали;
- скриншот или вывод терминала, если это поможет разобраться в результате;
- вопросы или описание проблемы, если что-то не получилось.

## Дополнительно: fork

Fork полезен в open-source или в ситуациях, когда у участника нет прав на запись в исходный репозиторий.

В этом курсе fork — дополнительное понятие, а не основной способ выполнения домашних заданий. Основной процесс — работа в личных ветках студента внутри homework repository.

# Модуль 06: ООП — основы

## Главное в модуле

- объекты, классы и конструкторы;
- поля, методы и инкапсуляция;
- композиция небольших объектов.

## Минимальный результат

После модуля студент должен уметь:

- создать простой класс TypeScript;
- скрыть внутренние данные и открыть понятные методы;
- объяснить, когда композиция полезнее наследования.


## Цель модуля

Цель модуля — понять базовые понятия объектно-ориентированного программирования (ООП), полезные в проектах автоматизации. Материал опирается на основы JavaScript и TypeScript из модулей 03–04 и работу с `async`/`await` и Promise из модуля 05.

После модуля вы сможете:

- объяснить, что такое class и object;
- создать простой class в TypeScript и объект через `new`;
- использовать constructor, properties и methods;
- на базовом уровне применять `public`, `private` и `readonly`;
- понимать простую composition и читать простой пример inheritance;
- объяснить, почему ООП пригодится при изучении Page Object Model.

## Почему ООП важно для Automation QA

В проекте автоматизации часто повторяются данные и действия. Classes помогают собрать связанные данные и поведение в одном месте, а вспомогательные объекты и похожие на сервисы classes делают тесты понятнее. Page Object Model также обычно использует classes. Базовое знание ООП помогает не превращать растущий проект в набор разрозненных функций и переменных.

## Темы модуля

- object и class;
- properties, methods и constructor;
- создание объектов через `new`;
- `public`, `private` и `readonly`;
- простая инкапсуляция;
- composition;
- обзор inheritance;
- ошибки применения ООП в автоматизации тестирования;
- связь с будущим Page Object Model.

## 1. Что такое объект

Object — структура данных, которая содержит связанные поля и значения.

```ts
const user = {
  email: 'qa.user@example.com',
  role: 'viewer',
  isActive: true,
};
```

Этот object описывает одного пользователя. Поля хранят данные именно об этом пользователе.

## 2. Что такое class

Class — шаблон для создания объектов.

```ts
class TestUser {
  email: string;
  role: string;
  isActive: boolean;

  constructor(email: string, role: string, isActive: boolean) {
    this.email = email;
    this.role = role;
    this.isActive = isActive;
  }
}

const user = new TestUser('qa.user@example.com', 'viewer', true);
```

`class TestUser` описывает данные тестового пользователя, `new TestUser(...)` создаёт объект этого класса, а `this` указывает на текущий объект.

## 3. Constructor

Constructor запускается при создании нового объекта, получает начальные данные и подготавливает состояние объекта.

```ts
class TestCase {
  id: string;
  title: string;
  priority: string;

  constructor(id: string, title: string, priority: string) {
    this.id = id;
    this.title = title;
    this.priority = priority;
  }
}

const loginTest = new TestCase(
  'TC-001',
  'User can log in with valid credentials',
  'high',
);
```

## 4. Properties

Properties — данные, которые хранятся внутри объекта.

```ts
class BrowserConfig {
  browserName: string;
  isHeadless: boolean;
  timeoutMs: number;

  constructor(browserName: string, isHeadless: boolean, timeoutMs: number) {
    this.browserName = browserName;
    this.isHeadless = isHeadless;
    this.timeoutMs = timeoutMs;
  }
}
```

`browserName`, `isHeadless` и `timeoutMs` — properties класса.

## 5. Methods

Methods — функции внутри класса. Они описывают поведение, связанное с объектом.

```ts
class TestUser {
  email: string;
  role: string;
  isActive: boolean;

  constructor(email: string, role: string, isActive: boolean) {
    this.email = email;
    this.role = role;
    this.isActive = isActive;
  }

  getDisplayName(): string {
    return `${this.email} (${this.role})`;
  }

  canLogin(): boolean {
    return this.isActive;
  }
}

const user = new TestUser('qa.user@example.com', 'viewer', true);

console.log(user.getDisplayName());
console.log(user.canLogin());
```

`getDisplayName` и `canLogin` — methods. Через `this` они используют properties текущего объекта.

## 6. Краткая запись constructor в TypeScript

В TypeScript часто встречается краткая запись:

```ts
class TestUser {
  constructor(
    public email: string,
    public role: string,
    public isActive: boolean,
  ) {}

  canLogin(): boolean {
    return this.isActive;
  }
}
```

Так TypeScript одновременно создаёт properties и присваивает им значения. Эта запись короче и распространена в проектах, но для первого знакомства полная запись из предыдущих примеров нагляднее.

## 7. `public`, `private` и `readonly`

- `public` разрешает обращаться к property или method извне класса;
- `private` ограничивает использование пределами класса;
- `readonly` позволяет присвоить значение при создании, но запрещает менять его позднее.

```ts
class TestUser {
  constructor(
    public readonly email: string,
    private password: string,
    public role: string,
  ) {}

  checkPassword(password: string): boolean {
    return this.password === password;
  }
}

const user = new TestUser('qa.user@example.com', 'Password123!', 'viewer');

console.log(user.email);
console.log(user.role);
console.log(user.checkPassword('Password123!'));
```

`email` доступен для чтения извне, а `password` является private. Внешний код не должен обращаться к `password` напрямую. Здесь речь идёт об организации кода и инкапсуляции, а не о полноценной защите секретов.

## 8. Инкапсуляция

Инкапсуляция означает, что объект хранит внутренние детали внутри и открывает другому коду только необходимое.

```ts
class LoginCredentials {
  constructor(
    private email: string,
    private password: string,
  ) {}

  getEmail(): string {
    return this.email;
  }

  isPasswordValid(password: string): boolean {
    return this.password === password;
  }
}
```

Внешнему коду не нужно знать, как хранится пароль: для работы с объектом он использует methods.

## 9. Composition

Composition означает, что один class использует внутри объект другого класса.

```ts
class TestUser {
  constructor(
    public email: string,
    public role: string,
  ) {}
}

class TestRun {
  constructor(
    public testName: string,
    public user: TestUser,
  ) {}

  getDescription(): string {
    return `${this.testName} is executed by ${this.user.email}`;
  }
}

const user = new TestUser('qa.user@example.com', 'viewer');
const testRun = new TestRun('Login test', user);

console.log(testRun.getDescription());
```

`TestRun` содержит `TestUser`. Такой подход часто безопаснее и проще inheritance. Composition особенно полезна в автоматизации тестирования, где разные небольшие объекты совместно решают задачу.

## 10. Inheritance: только первое знакомство

Inheritance позволяет одному class расширить другой.

```ts
class BaseTestUser {
  constructor(public email: string) {}

  getEmail(): string {
    return this.email;
  }
}

class AdminUser extends BaseTestUser {
  canManageUsers(): boolean {
    return true;
  }
}

const admin = new AdminUser('admin@example.com');

console.log(admin.getEmail());
console.log(admin.canManageUsers());
```

Inheritance бывает полезно, но при чрезмерном использовании усложняет код. Для этого курса достаточно уметь читать простое наследование. Обычно мы будем предпочитать небольшие classes и composition.

## 11. Static methods

Static method принадлежит самому классу, а не созданному объекту.

```ts
class EmailFactory {
  static createEmail(prefix: string): string {
    return `${prefix}@example.com`;
  }
}

const email = EmailFactory.createEmail('qa.user');
```

Создавать `new EmailFactory()` не требуется. Это удобно для простых вспомогательных methods, но злоупотреблять static methods не стоит.

## 12. Как это связано с будущим Page Object Model

В Page Object Model страницу часто представляет class, действия страницы становятся methods, а зависимости передаются через constructor. Знания из этого модуля подготовят вас к чтению такой структуры.

```ts
// Предварительный пример для следующих модулей
class LoginPage {
  constructor(private page: unknown) {}

  async open(): Promise<void> {
    // Логика открытия страницы будет добавлена позднее
  }
}
```

Это только предварительный пример. Настоящий Page Object Model с Playwright будет рассмотрен в следующих модулях.

## 13. Типичные ошибки

- создавать class там, где достаточно простого объекта;
- собирать несвязанные methods в одном class;
- слишком рано применять inheritance;
- делать всё `public`;
- делать всё `private` без причины;
- забывать `this` при обращении к данным объекта;
- создавать methods, которые не используют данные класса;
- излишне усложнять простые тестовые данные.

## Что обязательно понять в этом модуле

- class и object;
- constructor, properties и methods;
- значение `this`;
- создание объекта через `new`;
- базовое назначение `public` и `private`;
- composition.

## Что пока достаточно узнать обзорно

- inheritance;
- static methods;
- связь с будущим Page Object Model.

## Практика на занятии

1. Откройте репозиторий домашних заданий `Pw-Ts-Qa-Hw` в VS Code.
2. Проверьте текущую ветку.
3. Переключитесь на личную ветку `master` и получите обновления.
4. Переключитесь на ветку модуля 06.
5. Перед началом работы добавьте в неё изменения из личной ветки `master`.
6. Создайте файлы упражнений по ООП.
7. Создайте простые classes.
8. Добавьте constructors.
9. Добавьте properties и methods.
10. Используйте `public`, `private` и `readonly`.
11. Создайте пример composition.
12. Добавьте один простой пример inheritance.
13. Запустите typecheck.

```bash
git fetch origin

git switch student/{student-name-slug}/master
git pull origin student/{student-name-slug}/master

git switch student/{student-name-slug}/module-06-oop-basics
git merge origin/student/{student-name-slug}/master
git push
```

```bash
npm run typecheck
```

## Вопросы для проверки понимания

1. Что такое object?
2. Что такое class?
3. Чем object отличается от class?
4. Для чего нужен constructor?
5. Что такое properties?
6. Что такое methods?
7. На какой объект указывает `this`?
8. Что делает `new`?
9. Что означает `public`?
10. Что означает `private`?
11. Что означает `readonly`?
12. Что такое инкапсуляция?
13. Что такое composition?
14. Почему composition полезна в автоматизации тестирования?
15. Что такое inheritance?
16. Почему inheritance не следует использовать без необходимости?
17. Что такое static method?
18. Как ООП поможет позднее понять Page Object Model?
19. Какие ошибки применения ООП часто встречаются в автоматизации тестирования?

## Краткий итог

Теперь вы понимаете основы ООП, можете создать простой class, добавить constructor, properties и methods и использовать `this`. Вы познакомились с базовой инкапсуляцией и composition, получили обзор inheritance и подготовились к будущему изучению Page Object Model.

# Этап 1. Основы TypeScript

[← К обзору](README.md) · [Этап 2 →](02-nodejs.md)

**Цель:** научиться уверенно использовать TypeScript во frontend и backend.

Упражнения пишем в `server/playground/` (например, `1-1-basics.ts`).
Запуск — `node playground/1-1-basics.ts`, проверка типов — `npx tsc -p playground`
(обе команды из папки `server`). Ошибки типов также подсвечивает редактор.

Важно понимать разницу: `node` **не проверяет типы**, он просто удаляет их и
выполняет JavaScript. Типы проверяет только `tsc` (и редактор).

## Задачи

- [x] [1.1. Типы, аннотации и вывод типов](#11-типы-аннотации-и-вывод-типов)
- [x] [1.2. Объекты и массивы: `type` и `interface`](#12-объекты-и-массивы-type-и-interface)
- [ ] [1.3. Union и literal types](#13-union-и-literal-types)
- [ ] [1.4. Функции](#14-функции)
- [ ] [1.5. Narrowing](#15-narrowing)
- [ ] [1.6. `unknown` и `any`](#16-unknown-и-any)
- [ ] [1.7. Generics](#17-generics)
- [ ] [1.8. Асинхронные функции](#18-асинхронные-функции)
- [ ] [1.9. Итог: типы предметной области StatusHub](#19-итог-типы-предметной-области-statushub)

---

## 1.1. Типы, аннотации и вывод типов

**Что изучаем.** TypeScript — это JavaScript с описанием типов, которое
проверяется до запуска. Базовые типы: `string`, `number`, `boolean`, `null`,
`undefined`. Аннотация (`let port: number = 3000`) явно задаёт тип, но чаще
TypeScript выводит тип сам (`let port = 3000`). Нужно понимать, когда
аннотация полезна, а когда лишняя.

**Задание.**

1. Создать `playground/1-1-basics.ts`.
2. Объявить несколько переменных с аннотациями и без. Навести курсор в
   редакторе и посмотреть, какой тип вывел TypeScript.
3. Сравнить вывод типа для `let status = 'up'` и `const status = 'up'`.
4. Специально допустить ошибку типа, увидеть её в редакторе и в `tsc`,
   затем запустить файл через `node` и убедиться, что он всё равно выполняется.

**Критерии готовности.**

- `npx tsc -p playground` проходит без ошибок (после исправления намеренной ошибки).
- Можете показать пример, где аннотация не нужна, и пример, где нужна.

**Вопросы для самопроверки.**

- Почему код с ошибкой типов выполняется через `node`?
- Чем отличается тип `let x = 'up'` от `const x = 'up'`?
- В каких местах аннотации стоит писать всегда, а где лучше довериться выводу?

**Документация.**

- [TypeScript for JavaScript Programmers](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)
- [Everyday Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)
- [Node.js: Running TypeScript Natively](https://nodejs.org/en/learn/typescript/run-natively)

---

## 1.2. Объекты и массивы: `type` и `interface`

**Что изучаем.** Как описывать форму объекта: обязательные и необязательные
(`?`) поля, `readonly`, массивы (`Site[]`). Два способа задать тип объекта —
`type` и `interface` — и чем они отличаются на практике.

**Задание.**

1. В `playground/1-2-objects.ts` описать тип `Site` — сайт для мониторинга:
   идентификатор, URL, название (необязательное), дата добавления.
2. Создать массив из нескольких сайтов и функцию, которая печатает их список.
3. Описать тот же тип через `interface` и сравнить.
4. Попробовать изменить `readonly`-поле и добавить в объект лишнее поле —
   разобрать, какие ошибки выдаёт TypeScript.

**Критерии готовности.**

- Тип `Site` описан, массив сайтов типизирован, файл проходит проверку типов.
- Выбран и обоснован один из вариантов (`type` или `interface`) для проекта.

**Вопросы для самопроверки.**

- Чем `name?: string` отличается от `name: string | undefined`?
  (Подсказка: в проекте включён `exactOptionalPropertyTypes`.)
- Защищает ли `readonly` объект во время выполнения программы?
- Что умеет `interface`, чего не умеет `type`, и наоборот?

**Документация.**

- [Object Types](https://www.typescriptlang.org/docs/handbook/2/objects.html)
- [Differences Between Type Aliases and Interfaces](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)

---

## 1.3. Union и literal types

**Что изучаем.** Union type (`A | B`) — значение одного из нескольких типов.
Literal type — конкретное значение как тип (`'up'`, `200`). Вместе они
позволяют описать ограниченный набор вариантов: `'up' | 'down'`.

В этом проекте `enum` недоступен (`erasableSyntaxOnly`), и union из литералов —
его стандартная замена.

**Задание.**

1. В `playground/1-3-unions.ts` описать тип `CheckStatus` — возможные состояния
   проверки сайта.
2. Написать функцию, которая по статусу возвращает текст для пользователя.
3. Описать тип для HTTP-метода, которым будем проверять сайт (`'GET' | 'HEAD'`).
4. Попробовать написать `enum` и разобрать, почему проверка типов его не пропускает.

**Критерии готовности.**

- Передача недопустимого значения статуса вызывает ошибку типов.
- Понятно, почему в проекте вместо `enum` используем union.

**Вопросы для самопроверки.**

- Чем `string` отличается от `'up' | 'down'` с точки зрения безопасности?
- Почему `enum` нельзя просто «стереть», как остальные типы?
- Какой тип получится у `string | number`, и что с ним можно делать без проверок?

**Документация.**

- [Union Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types)
- [Literal Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types)
- [`erasableSyntaxOnly`](https://www.typescriptlang.org/tsconfig/#erasableSyntaxOnly)

---

## 1.4. Функции

**Что изучаем.** Типизация параметров и возвращаемого значения, необязательные
параметры и значения по умолчанию, тип `void`, функции как значения
(тип функции, колбэки).

**Задание.**

1. В `playground/1-4-functions.ts` написать функцию `isValidUrl(input: string): boolean`
   с помощью встроенного класса `URL`. Допустимы только `http:` и `https:`.
2. Написать функцию `formatResponseTime(ms, unit?)` с параметром по умолчанию.
3. Написать функцию, которая принимает массив URL и колбэк, вызываемый для
   каждого валидного URL. Описать тип колбэка.

**Критерии готовности.**

- `isValidUrl` правильно обрабатывает примеры: `https://example.com`,
  `ftp://example.com`, `example.com`, пустую строку.
- У всех публичных функций явно указаны типы параметров и результата.

**Вопросы для самопроверки.**

- Почему `new URL('example.com')` бросает ошибку, и как это учесть?
- Зачем явно указывать тип результата, если TypeScript его выводит?
- Как описать тип функции, которая принимает строку и ничего не возвращает?

**Документация.**

- [More on Functions](https://www.typescriptlang.org/docs/handbook/2/functions.html)
- [MDN: URL](https://developer.mozilla.org/en-US/docs/Web/API/URL)

---

## 1.5. Narrowing

**Что изучаем.** Narrowing — сужение типа внутри условия: после проверки
`typeof x === 'string'` TypeScript знает, что `x` — строка. Способы: `typeof`,
`in`, `instanceof`, сравнение с литералом. Особенно важен **discriminated
union** — объединение объектов с общим полем-признаком.

**Задание.**

1. В `playground/1-5-narrowing.ts` описать результат проверки сайта как
   discriminated union из двух вариантов:
   - сайт ответил — есть HTTP-статус и время ответа;
   - не удалось получить ответ — есть описание ошибки.
2. Написать функцию, которая выводит результат в консоль по-разному для
   каждого варианта.
3. Добавить третий вариант (например, таймаут) и посмотреть, где TypeScript
   подскажет, что код нужно обновить. Разобрать приём с `never`.

**Критерии готовности.**

- Внутри каждой ветки доступны только поля соответствующего варианта.
- Добавление нового варианта приводит к ошибке типов там, где он не обработан.

**Вопросы для самопроверки.**

- Что такое поле-дискриминант и почему оно должно быть литеральным типом?
- Почему этот подход лучше, чем один объект со всеми полями необязательными?
- Как `never` помогает не забыть обработать новый вариант?

**Документация.**

- [Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)
- [Discriminated Unions](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#discriminated-unions)
- [Exhaustiveness Checking](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#exhaustiveness-checking)

---

## 1.6. `unknown` и `any`

**Что изучаем.** Данные извне (JSON, тело HTTP-запроса, файл) могут быть
чем угодно. `any` отключает проверку типов, `unknown` — честно говорит
«неизвестно» и требует проверки перед использованием. Type guard — функция,
которая проверяет значение и сужает тип (`value is T`).

**Задание.**

1. В `playground/1-6-unknown.ts` получить данные через `JSON.parse` и
   посмотреть, какой у них тип.
2. Написать type guard `isCheckRequest(value: unknown): value is CheckRequest`,
   где `CheckRequest` — объект с полем `url: string`.
3. Проверить guard на корректных и некорректных входных данных: `null`,
   строка, объект без `url`, объект с `url: 123`.
4. Сравнить с вариантом через `any` и найти ошибку, которую `any` пропускает.

**Критерии готовности.**

- Guard корректно отвергает все некорректные варианты.
- Нигде в упражнении не используется `any` (кроме сравнения в п. 4).

**Вопросы для самопроверки.**

- Почему `JSON.parse` возвращает `any`, и чем это опасно?
- Чем `unknown` отличается от `any`?
- Гарантирует ли type guard корректность, если в нём ошибка? Кто за это отвечает?

**Документация.**

- [`unknown`](https://www.typescriptlang.org/docs/handbook/2/functions.html#unknown)
- [Using type predicates](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates)

---

## 1.7. Generics

**Что изучаем.** Generics — типы с параметрами. Позволяют написать одну
функцию или тип, который работает с разными данными, не теряя информацию о
типе: `Array<Site>`, `Promise<CheckResult>`. Ограничения параметра (`extends`).

**Задание.**

1. В `playground/1-7-generics.ts` написать функцию `first<T>(items: T[])`,
   возвращающую первый элемент. Разобрать тип результата (учитывая
   `noUncheckedIndexedAccess`).
2. Описать generic-тип ответа API `ApiResponse<T>`: успешный ответ с данными
   типа `T` или ответ с ошибкой.
3. Использовать его для двух разных случаев: список сайтов и результат проверки.

**Критерии готовности.**

- TypeScript правильно выводит `T` без явного указания при вызове.
- `ApiResponse<T>` — discriminated union, и narrowing работает по признаку.

**Вопросы для самопроверки.**

- Чем `first<T>(items: T[]): T | undefined` лучше, чем `first(items: any[]): any`?
- Почему результат `first` может быть `undefined`?
- Когда нужен `extends` в параметре типа?

**Документация.**

- [Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)
- [`noUncheckedIndexedAccess`](https://www.typescriptlang.org/tsconfig/#noUncheckedIndexedAccess)

---

## 1.8. Асинхронные функции

**Что изучаем.** Как типизировать асинхронный код: `Promise<T>`, что
возвращает `async`-функция, как TypeScript выводит тип после `await`.
Здесь — только типизация; устройство асинхронности подробно разберём в этапе 2.

**Задание.**

1. В `playground/1-8-async.ts` написать функцию `sleep(ms: number): Promise<void>`.
2. Написать заглушку `checkSite(url: string): Promise<CheckResult>`, которая
   ждёт случайное время и возвращает случайный результат (тип из задачи 1.5).
3. Вызвать её для нескольких URL последовательно и параллельно
   (`Promise.all`), сравнить общее время.

**Критерии готовности.**

- Тип результата `checkSite` явно указан как `Promise<CheckResult>`.
- Разница во времени между последовательным и параллельным запуском видна в выводе.

**Вопросы для самопроверки.**

- Какой тип возвращает `async`-функция, в которой написано `return 42`?
- Что будет, если забыть `await` перед вызовом `checkSite`? Поймает ли это TypeScript?
- Какой тип у результата `Promise.all([checkSite(a), checkSite(b)])`?

**Документация.**

- [MDN: async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [MDN: Promise.all](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)

---

## 1.9. Итог: типы предметной области StatusHub

**Что изучаем.** Собираем всё вместе: описываем типы, которые будут
использоваться в настоящем проекте.

**Задание.**

1. Создать `server/src/types.ts` с типами: сайт, запрос на проверку,
   результат проверки (discriminated union), ответ API.
2. Экспортировать их и импортировать в `server/src/index.ts` через `import type`
   (пока без использования — только чтобы проверить импорт).
3. Обсудить с наставником: какие решения приняты и почему.

**Критерии готовности.**

- `npm run typecheck` в папке `server` проходит.
- Сервер по-прежнему запускается через `npm run dev`.
- Каждое поле типов можно объяснить: зачем оно и почему у него такой тип.

**Вопросы для самопроверки.**

- Почему в проекте нужен именно `import type`, а не обычный `import`?
  (Подсказка: `verbatimModuleSyntax` и удаление типов в Node.js.)
- Какие поля результата проверки могут понадобиться позже (история, БД)?
- Где эти типы понадобятся на клиенте, и как их туда передать? (Вернёмся к этому в этапе 4.)

**Документация.**

- [Modules: Importing Types](https://www.typescriptlang.org/docs/handbook/2/modules.html#typescript-specific-es-module-syntax)
- [`verbatimModuleSyntax`](https://www.typescriptlang.org/tsconfig/#verbatimModuleSyntax)

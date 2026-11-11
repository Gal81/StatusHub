# Этап 2. Основы Node.js

[← Этап 1](01-typescript.md) · [К обзору](README.md) · [Этап 3 →](03-backend-http.md)

**Цель:** понять, как работает JavaScript за пределами браузера.

Упражнения — в `server/playground/`, запуск из папки `server`:
`node playground/<файл>.ts`.

## Задачи

- [ ] [2.1. Что такое Node.js](#21-что-такое-nodejs)
- [ ] [2.2. Модули и импорты](#22-модули-и-импорты)
- [ ] [2.3. npm и package.json](#23-npm-и-packagejson)
- [ ] [2.4. Promise и async/await](#24-promise-и-asyncawait)
- [ ] [2.5. Event loop на базовом уровне](#25-event-loop-на-базовом-уровне)
- [ ] [2.6. Работа с файлами](#26-работа-с-файлами)
- [ ] [2.7. Переменные окружения](#27-переменные-окружения)
- [ ] [2.8. Итог: консольная проверка сайтов](#28-итог-консольная-проверка-сайтов)

---

## 2.1. Что такое Node.js

**Что изучаем.** Node.js — среда выполнения JavaScript вне браузера (движок V8
плюс системные API: файлы, сеть, процессы). Чего в Node.js нет (`window`,
`document`, DOM) и что есть взамен (`process`, `globalThis`, модули `node:*`).

**Задание.**

1. В `playground/2-1-hello.ts` вывести: версию Node.js, текущую папку,
   платформу, аргументы командной строки (`process.argv`).
2. Попробовать обратиться к `window` и `document` — разобрать результат.
3. Написать скрипт, который принимает имя через аргумент и печатает приветствие;
   если аргумента нет — завершается с кодом `1` и сообщением об ошибке.

**Критерии готовности.**

- Скрипт работает: `node playground/2-1-hello.ts Иван`.
- Без аргумента команда `echo $?` после запуска показывает `1`.

**Вопросы для самопроверки.**

- Что общего у JavaScript в браузере и в Node.js, а что разное?
- Что лежит в `process.argv[0]` и `process.argv[1]`?
- Зачем программе код завершения, и кто его читает?

**Документация.**

- [Introduction to Node.js](https://nodejs.org/en/learn/getting-started/introduction-to-nodejs)
- [Differences between Node.js and the Browser](https://nodejs.org/en/learn/getting-started/differences-between-nodejs-and-the-browser)
- [process](https://nodejs.org/api/process.html)

---

## 2.2. Модули и импорты

**Что изучаем.** ES-модули: `import`/`export`, именованный экспорт и экспорт
по умолчанию, встроенные модули с префиксом `node:`. Почему в `package.json`
указано `"type": "module"`. Как Node.js находит модули (относительные пути с
расширением, пакеты из `node_modules`).

**Задание.**

1. Вынести `isValidUrl` из этапа 1 в `playground/lib/url.ts` и импортировать
   в другой файл.
2. Импортировать встроенный модуль (например, `node:os`) и вывести пару значений.
3. Разобрать, почему в импорте нужно указывать расширение `.ts`.
4. Посмотреть, как импортируется `fastify` в `server/src/index.ts`, и найти
   этот пакет в `node_modules`.

**Критерии готовности.**

- Импорт между своими файлами работает и при запуске, и при проверке типов.

**Вопросы для самопроверки.**

- Чем `import { x } from './a.ts'` отличается от `import x from './a.ts'`?
- Откуда Node.js берёт `fastify`, если путь не указан?
- Что изменится, если убрать `"type": "module"` из `package.json`?

**Документация.**

- [Node.js: ECMAScript modules](https://nodejs.org/api/esm.html)
- [Node.js: Modules — TypeScript](https://nodejs.org/api/typescript.html)

---

## 2.3. npm и package.json

**Что изучаем.** Что такое пакет и npm. Поля `package.json`: `scripts`,
`dependencies`, `devDependencies`, `engines`. Семантическое версионирование
(`^5.12.8`). Зачем `package-lock.json` и чем `npm ci` отличается от `npm install`.

**Задание.**

1. Разобрать `server/package.json` и `client/package.json` построчно.
2. Добавить в `server/package.json` скрипт для запуска playground-файла и
   проверить, что он работает через `npm run`.
3. Командой `npm ls` посмотреть дерево зависимостей сервера.
4. Объяснить, какие версии fastify допускает запись `^5.12.8`.

**Критерии готовности.**

- Можете объяснить каждое поле обоих `package.json`.
- Свой скрипт запускается через `npm run`.

**Вопросы для самопроверки.**

- Почему `typescript` — в `devDependencies`, а `fastify` — в `dependencies`?
- Зачем коммитить `package-lock.json`, но не `node_modules`?
- Когда использовать `npm ci`, а когда `npm install`?

**Документация.**

- [package.json](https://docs.npmjs.com/cli/configuring-npm/package-json)
- [About semantic versioning](https://docs.npmjs.com/about-semantic-versioning)
- [npm ci](https://docs.npmjs.com/cli/commands/npm-ci)

---

## 2.4. Promise и async/await

**Что изучаем.** Promise — объект, представляющий результат, который появится
позже: состояния pending / fulfilled / rejected. `then`/`catch` и `async`/`await`
как два способа работы с ним. Обработка ошибок через `try/catch`.
`Promise.all` и `Promise.allSettled`.

**Задание.**

1. В `playground/2-4-promises.ts` создать Promise вручную через `new Promise`,
   который выполняется или отклоняется в зависимости от аргумента.
2. Обработать его двумя способами: через `then/catch` и через `async/await`.
3. Запустить несколько промисов, один из которых отклоняется, через
   `Promise.all` и через `Promise.allSettled` — сравнить поведение.
4. Посмотреть, что произойдёт с отклонённым промисом без обработки ошибки.

**Критерии готовности.**

- Все варианты работают, и разница между `all` и `allSettled` объяснена.

**Вопросы для самопроверки.**

- Что возвращает `async`-функция, если внутри неё бросить ошибку?
- Почему для проверки нескольких сайтов `allSettled` может подойти лучше, чем `all`?
- Что такое unhandled rejection и чем он опасен в сервере?

**Документация.**

- [MDN: Using promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
- [MDN: Promise.allSettled](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)

---

## 2.5. Event loop на базовом уровне

**Что изучаем.** Node.js выполняет JavaScript в одном потоке. Долгие операции
(сеть, файлы, таймеры) выполняются «в фоне», а их колбэки ставятся в очередь.
Event loop берёт задачи из очередей, когда стек вызовов пуст. Микрозадачи
(Promise) выполняются раньше таймеров. Почему тяжёлый синхронный код
блокирует весь сервер.

**Задание.**

1. В `playground/2-5-event-loop.ts` написать код с `console.log`,
   `setTimeout(..., 0)`, `Promise.resolve().then(...)` и `queueMicrotask`.
   **Сначала** записать предполагаемый порядок вывода, затем запустить и сравнить.
2. Написать «тяжёлый» синхронный цикл на 2–3 секунды и показать, что таймер
   на 100 мс срабатывает только после него.

**Критерии готовности.**

- Порядок вывода в п. 1 объяснён для каждой строки.
- Можете объяснить, почему таймер в п. 2 опоздал.

**Вопросы для самопроверки.**

- Почему `setTimeout(fn, 0)` не выполняется сразу?
- Что выполнится раньше: колбэк `then` или колбэк `setTimeout(0)`? Почему?
- Что случится с HTTP-сервером, если обработчик запроса выполняет долгий синхронный цикл?

**Документация.**

- [The Node.js Event Loop](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick)
- [Don't Block the Event Loop](https://nodejs.org/en/learn/asynchronous-work/dont-block-the-event-loop)
- [MDN: Microtask guide](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide)

---

## 2.6. Работа с файлами

**Что изучаем.** Модуль `node:fs/promises`: чтение и запись файлов,
кодировки, обработка ошибок (`ENOENT` — файл не найден). Сериализация данных
в JSON и обратно. Пути: относительные пути считаются от текущей папки, а не
от файла скрипта; `import.meta.dirname`.

**Задание.**

1. В `playground/2-6-files.ts` написать функции `loadSites()` и `saveSites(sites)`,
   которые хранят массив `Site` в `playground/data/sites.json`.
2. Если файла нет — `loadSites` возвращает пустой массив, а не падает.
3. Если файл содержит невалидный JSON — понятная ошибка.
4. Проверить данные из файла с помощью type guard из задачи 1.6.
5. Добавить `playground/data/` в `server/.gitignore`.

**Критерии готовности.**

- Сценарий «загрузить → добавить сайт → сохранить → загрузить снова» работает.
- Скрипт работает при запуске из разных папок.

**Вопросы для самопроверки.**

- Чем `readFile` из `node:fs/promises` отличается от `readFileSync`, и почему
  в сервере предпочтительнее первый?
- Почему результат `JSON.parse` нужно проверять?
- Какие проблемы появятся, если два процесса одновременно пишут в один файл?
  (Вернёмся к этому в этапе 6.)

**Документация.**

- [fs Promises API](https://nodejs.org/api/fs.html#promises-api)
- [import.meta.dirname](https://nodejs.org/api/esm.html#importmetadirname)

---

## 2.7. Переменные окружения

**Что изучаем.** Переменные окружения — настройки, которые передаются
программе снаружи, а не зашиты в код: порт, адрес БД, режим работы.
`process.env`, значения всегда строки (или `undefined`). Файл `.env` и флаг
`--env-file`. Почему секреты не коммитят.

**Задание.**

1. Сделать так, чтобы порт и host сервера в `server/src/index.ts` брались из
   `PORT` и `HOST` со значениями по умолчанию `3000` и `127.0.0.1`.
2. Некорректный `PORT` (например, `abc`) должен давать понятную ошибку при
   старте, а не странное поведение.
3. Запустить сервер с другим портом: `PORT=4000 npm run dev`.
4. Создать `server/.env.example` с описанием переменных; `.env` добавить в
   `.gitignore`. Попробовать запуск с `--env-file`.

**Критерии готовности.**

- Сервер стартует с портом по умолчанию и с портом из переменной.
- При `PORT=abc` — понятное сообщение и код выхода `1`.

**Вопросы для самопроверки.**

- Почему `process.env.PORT` имеет тип `string | undefined`, а не `number`?
- Чем `.env` отличается от `.env.example`, и что из них коммитить?
- Почему настройки лучше передавать через окружение, а не менять в коде?

**Документация.**

- [process.env](https://nodejs.org/api/process.html#processenv)
- [Node.js: `--env-file`](https://nodejs.org/api/cli.html#--env-fileconfig)
- [The Twelve-Factor App: Config](https://12factor.net/config)

---

## 2.8. Итог: консольная проверка сайтов

**Что изучаем.** Собираем этап вместе и делаем первую версию главной функции
StatusHub — пока в виде консольной программы. Знакомимся со встроенным
`fetch` и измерением времени через `performance.now()`. Подробно HTTP
разберём в этапе 3.

**Задание.**

1. `playground/check-cli.ts` читает список URL из файла, путь к которому
   передан аргументом.
2. Для каждого URL выполняет запрос через `fetch` и печатает: доступен ли
   сайт, HTTP-статус, время ответа в мс.
3. Если сайт недоступен (несуществующий домен) — программа не падает, а
   печатает ошибку для этого URL и продолжает.
4. Проверки выполняются параллельно.
5. Используются типы из `server/src/types.ts` (задача 1.9).

**Критерии готовности.**

- Работает на списке из рабочих, несуществующих и некорректных URL.
- Можете объяснить, какие части программы асинхронные и почему.

**Вопросы для самопроверки.**

- Что возвращает `fetch` для ответа `404`: ошибку или обычный результат?
- Почему для измерения времени лучше `performance.now()`, а не `Date.now()`?
- Что будет, если один из сайтов отвечает 2 минуты? (Решим в этапе 3.)

**Документация.**

- [Node.js: Fetch](https://nodejs.org/en/learn/getting-started/fetch)
- [MDN: Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [MDN: performance.now()](https://developer.mozilla.org/en-US/docs/Web/API/Performance/now)

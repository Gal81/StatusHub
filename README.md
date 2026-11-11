# StatusHub

Учебный full-stack проект: React + Vite + TypeScript и Node.js + Fastify.
Без базы данных и дополнительных сервисов.

Учебный план, задачи и прогресс: [docs/learning](docs/learning/README.md).

## Требования

Node.js 22.18+ и npm. Для нового окружения рекомендуется актуальная LTS-версия
Node.js.

## Установка зависимостей

Из корня проекта:

```sh
npm ci --prefix client
npm ci --prefix server
```

## Разработка

В первом терминале:

```sh
cd client
npm run dev
```

Vite покажет адрес клиента, по умолчанию http://localhost:5173.
Изменения React-компонентов обновляются через HMR.

Во втором терминале, начиная из корня проекта:

```sh
cd server
npm run dev
```

Сервер доступен на http://127.0.0.1:3000.
Node.js исполняет TypeScript напрямую и перезапускает сервер при изменении файлов.
Этот запуск не проверяет типы; для проверки используйте `npm run typecheck`
в папке `server`.

Проверка API:

```sh
curl http://127.0.0.1:3000/health
```

Ожидаемый ответ: `{"status":"ok"}`.

Клиент и сервер пока независимы: запросы из интерфейса к API не добавлены.
Остановка каждого приложения: Ctrl+C в соответствующем терминале.

## Сборка и проверки

Из корня проекта:

```sh
npm run lint --prefix client
npm run build --prefix client
npm run typecheck --prefix server
npm run build --prefix server
```

Сборки создаются в `client/dist` и `server/dist`.
Скомпилированный сервер запускается командой `npm start --prefix server`.
Сборку клиента можно локально посмотреть через `npm run preview --prefix client`.

## Структура

- `client/src/main.tsx` — входная точка React.
- `client/src/App.tsx` — корневой компонент.
- `client/src/index.css` — базовые стили.
- `client/vite.config.ts` — конфигурация Vite.
- `client/tsconfig*.json` — настройки TypeScript для браузера и Vite.
- `server/src/index.ts` — Fastify-сервер и маршрут `/health`.
- `server/tsconfig.json` — строгая проверка типов и сборка в JavaScript.
- В каждой папке свой `package.json` со скриптами и `package-lock.json`
  для воспроизводимой установки зависимостей.

# 🧱 Backend Golang Microservices Boilerplate

[![MIT License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

Это шаблон для учебных backend-проектов на Python с попыткой приблизиться к продакшн-ready подходу, в котором содержится чёткое разделение слоёв и подготовленная архитектура.

---

## 🎯 Цели проекта

- Упростить командную разработку микросервисов с единым подходом к архитектуре
- Обеспечить изоляцию логики каждого сервиса и независимость его запуска
- Ознакомить разработчиков с принципами разделения слоёв и хорошими практиками

---

## 📦 Общая структура репозитория

```
backend-go/
├── services/                            # 💡 Все микросервисы
│   └── abstract_service1/
│       ├── cmd/                         # 🚀 Точка входа
│       │   └── main.go
│       ├── internal/                    # 🔒 Внутренности сервиса
│       │   ├── config/                  # ⚙️ Конфиги (env, yaml, flags)
│       │   ├── app/                     # 🧠 Use-case'ы, обработчики
│       │   │   └── handler_factory.go   # DI / инициализация хендлеров
│       │   ├── domain/                  # 📐 Чистые сущности и интерфейсы
│       │   │   ├── model/               #     - Бизнес-структуры
│       │   │   ├── repository/          #     - Интерфейсы репозиториев
│       │   │   ├── events/              #     - Domain-события
│       │   │   └── service/             #     - Бизнес-интерфейсы (если есть)
│       │   ├── infrastructure/          # 🛠️ Реализация зависимостей
│       │   │   ├── persistence/         #     - ORM-модели и SQL реализация
│       │   │   │   ├── gorm_user.go
│       │   │   │   └── migrations/      # Миграции бд
│       │   │   │       └── 001_init.sql
│       │   │   ├── cache/               #     - Redis
│       │   │   ├── messaging/           #     - Kafka, RabbitMQ и т.п.
│       │   │   └── externalapi/         #     - вызовы в сторонние API
│       │   ├── transport/               # 🌐 Контроллеры и адаптеры
│       │   │   ├── http/
│       │   │   │   ├── handler/
│       │   │   │   │   └── user_handler.go
│       │   │   │   ├── middleware/
│       │   │   │   └── schemas/         #     - DTO/Request/Response
│       │   │   └── grpc/
│       │   │       └── grpc_server.go
│       |   |
│       │   ├── errors/                  # 🚨 Ошибки сервиса
│       │   │   ├── errdefs.go           #     - определение ошибок
│       │   │   └── error_mapper.go      #     - маппинг в HTTP/gRPC
|       |   |
│       │   ├── middleware/              # 🔐 Локальные middleware
│       │   |
│       │   ├── pkg/                     # 🔧 Утилиты специфичные для сервиса
│       │   │   └── time_utils.go
│       │   |
│       │   └── bootstrap/               # 🧩 DI и инициализация всех слоёв
│       │       └── container.go
│       |
│       ├── api/                         # 📄 Контракты (protobuf, openapi)
│       │   ├── protobuf/
│       │   └── openapi/
│       |
│       ├── deployments/                 # 🚀 Dockerfile и Helm charts
│       │   └── docker/
│       │       └── Dockerfile
│       |
│       ├── tests/                       # 🧪 Unit / интеграционные тесты
│       │   ├── unit/
│       │   └── integration/
│       |
│       ├── .env
│       ├── go.mod
│       ├── go.sum
│       └── README.md
│
├── pkg/                                 # 📦 Общие библиотеки и утилиты
│   ├── logger/                          #     - zap/logrus обёртки
│   ├── auth_middleware/                #     - JWT, RBAC
│   ├── config_loader/                  #     - чтение конфигов
│   ├── common_utils/                   #     - shared utils (string/date/etc.)
│   └── shared_errors/                  #     - общие error defs
|
├── api/                                 # 📄 Общие protobuf/openapi схемы
│   ├── common_protobuf/
│   └── common_openapi/
│
├── deployments/                         # 📦 Общая инфраструктура (docker-compose, helm, etc.)
│   └── docker-compose.yml
│
├── tests/                               # 🧪 e2e / контрактные тесты
│   ├── e2e_flows/
│   └── health_checks/
│
├── scripts/                             # 🛠️ CLI-утилиты, миграции и т.п.
│
├── go.work                              # 📦 go workspace
├── Makefile                             # 🧱 комманды
└── README.md
```

| Директория                             | Назначение                                                                                           |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| `cmd/`                                 | **Точка входа** в приложение (main.go). Подключение конфигов, логгера, DI, запуск сервера.           |
| `internal/app/`                        | **Бизнес-логика**: use-case'ы, сценарии, координация вызовов репозиториев и сервисов.                |
| `internal/domain/model/`               | **Чистые сущности**: структуры (User, Order…), без зависимостей от фреймворков.                      |
| `internal/domain/service/`             | Интерфейсы бизнес-сервисов, которые реализуются в `app/` (если нужно отделить бизнес-сервисы).       |
| `internal/domain/events/`              | **Domain-события**: структура событий, которые можно публиковать (UserCreated, PaymentFailed и т.д.) |
| `internal/domain/repository/`          | Интерфейсы репозиториев (например, `UserRepository`), без реализации.                                |
| `internal/infrastructure/persistence/` | **Реализация репозиториев**, ORM-модели, SQL-запросы, миграции.                                      |
| `internal/infrastructure/cache/`       | Работа с Redis, Memcached и прочими кэшами.                                                          |
| `internal/infrastructure/externalapi/` | Обёртки над внешними API — Stripe, Telegram, сторонние сервисы.                                      |
| `internal/infrastructure/messaging/`   | Продюсеры/консьюмеры Kafka, RabbitMQ и др.                                                           |
| `internal/transport/http/handler/`     | HTTP-хендлеры (контроллеры): принимают запросы, вызывают use-case’ы, отдают ответ.                   |
| `internal/transport/http/schemas/`     | DTO/Payload структуры запроса и ответа (аналог `schemas/` в FastAPI).                                |
| `internal/transport/http/middleware/`  | Middleware-обработчики: логгеры, авторизация, метрики.                                               |
| `internal/transport/grpc/`             | gRPC-сервер, реализация gRPC-хендлеров.                                                              |
| `internal/errors/`                     | Кастомные ошибки (например, `ErrUserNotFound`) и мапперы для HTTP/gRPC ошибок.                       |
| `internal/middleware/`                 | Middleware, общие для сервиса, но не для конкретного транспорта.                                     |
| `internal/config/`                     | Конфигурации приложения: структуры, загрузка из `.env`, yaml и т.п.                                  |
| `internal/bootstrap/`                  | DI: сборка и инициализация зависимостей, контейнер, wire/fx-интеграция.                              |
| `internal/pkg/`                        | Вспомогательные модули, хелперы, парсеры, валидаторы (только для этого сервиса).                     |
| `pkg/`                                 | **Общие для всех сервисов**: логгеры, middleware, shared utils, ошибки, общие интерфейсы.            |
| `api/`                                 | Контракты: `protobuf`, `OpenAPI`, gRPC-схемы и Swagger-документация.                                 |
| `deployments/`                         | Dockerfile, Helm charts и прочая инфраструктура для CI/CD.                                           |
| `scripts/`                             | CLI-утилиты, миграции, генераторы, линтеры.                                                          |
| `tests/unit/`                          | Unit-тесты.                                                                                          |
| `tests/integration/`                   | Интеграционные тесты, мок-серверы и фейковые зависимости.                                            |


Подробнее про слоистую архитектуру: [здесь](https://testengineer.ru/layered-architecture/)

---

## 📄 Лицензия

Проект распространяется под лицензией MIT. Используйте свободно.

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
│   ├── abstract_service1/
│   |   ├── cmd/                         # 🚀 Точка входа
│   |   │   └── main.go
│   |   ├── internal/                    # 🔒 Внутренности сервиса
│   |   │   ├── config/                  # ⚙️ Конфиги (env, yaml, flags)
|   |   |   |   ├── config.go
│   |   │   │   └── config.yaml
│   |   │   ├── app/                     # 🧠 Use-case'ы, обработчики
│   |   │   │   ├── abstract_service1.go
│   |   │   │   └── abstract_service2.go
│   |   │   ├── domain/                  # 📐 Чистые сущности и интерфейсы
│   |   │   │   ├── models/              #     - Бизнес-структуры
│   |   │   │   |   ├── abstract_model1.go
│   |   │   │   |   └── abstract_model2.go
│   |   │   │   ├── repositories/        #     - Интерфейсы репозиториев
│   |   │   │   |   ├── abstract_repository1.go
│   |   │   │   |   └── abstract_repository2.go
│   |   │   │   ├── events/              #     - Domain-события
│   |   │   │   |   ├── abstract_event1.go
│   |   │   │   |   └── abstract_event2.go
│   |   │   │   └── services/            #     - Бизнес-интерфейсы (если есть)
│   |   │   │       ├── abstract_service1.go
│   |   │   │       └── abstract_service2.go
│   |   │   ├── infrastructure/          # 🛠️ Реализация зависимостей
│   |   │   │   ├── cache/
│   |   │   │   |   └── redis.go
|   |   |   |   └── repository/
│   |   │   │       └── db.go
│   |   │   ├── transport/               # 🌐 Контроллеры и адаптеры
│   |   │   │   ├── http/
|   |   │   │   │   ├── handler/
│   |   │   │   │   │   ├── abstract_handler1.go
│   |   │   │   │   │   └── abstract_handler2.go
│   |   │   │   │   ├── middleware/
│   |   │   │   │   │   ├── abstract_middleware1.go
│   |   │   │   │   │   └── abstract_middleware2.go
│   |   │   │   │   └── schemas/         #     - DTO/Request/Response
│   |   │   │   │       ├── abstract_scheme1.go
│   |   │   │   │       └── abstract_scheme2.go
│   |   │   │   └── grpc/
│   |   │   │       └── grpc_server.go
│   |   |   |
│   |   │   ├── errors/                  # 🚨 Ошибки сервиса
│   |   │   │   ├── errdefs.go           #     - определение ошибок
│   |   │   │   └── error_mapper.go      #     - маппинг в HTTP/gRPC
|   |   |   |
│   |   │   ├── middleware/              # 🔐 Локальные middleware
│   |   │   │   ├── abstract_middleware1.go
│   |   │   │   └── abstract_middleware2.go
│   |   │   |
│   |   │   └── pkg/                     # 🔧 Утилиты специфичные для сервиса
│   |   │       ├── abstract_utility1.go
│   |   │       └── abstract_utility2.go
│   |   ├── migrations/              # Миграции бд
│   |   │   └── 001_init.sql
|   |   |
│   |   ├── api/                         # 📄 Контракты (protobuf, openapi)
│   |   │   ├── protobuf/
|   |   |   |   ├── abstract_protobuf1.yaml
│   |   │   |   └── abstract_protobuf2.yaml
│   |   │   └── openapi/
│   |   │       ├── abstract_openapi1.yaml
│   |   │       └── abstract_openapi2.yaml
│   |   |
│   |   ├── deployments/                 # 🚀 Dockerfile и Helm charts
│   |   │   └── docker/
│   |   │       └── Dockerfile
│   |   |
│   |   ├── tests/                       # 🧪 Unit / интеграционные тесты
│   |   │   ├── unit/
│   |   │   |   ├── abstract_test1.go
│   |   │   |   └── abstract_test2.go
│   |   │   └── integration/
│   |   │       ├── abstract_test1.go
│   |   │       └── abstract_test2.go
│   |   |
│   |   ├── .env
│   |   ├── go.mod
│   |   ├── go.sum
│   |   └── README.md
│   |
|   └── abstract_service2/               # Всё то же самое что и в service1/
|   
├── pkg/                                 # 📦 Общие библиотеки и утилиты
│   ├── auth_middleware/                 #     - JWT, RBAC
|   |   └── jwt.go
│   ├── config_loader/                   #     - чтение конфигов
|   |   └── loader.go
│   ├── common_utils/                    #     - shared utils (string/date/etc.)
|   |   ├── abstract_common_util1.go
|   |   └── abstract_common_util2.go
│   └── shared_errors/                   #     - общие error defs
|       ├── abstract_error1.go
|       └── abstract_error2.go
|
├── api/                                 # 📄 Общие protobuf/openapi схемы
│   ├── common_protobuf/
|   |   ├── abstract_protobuf1.yaml
│   |   └── abstract_protobuf2.yaml
│   └── common_openapi/
│       ├── abstract_openapi1.yaml
│       └── abstract_openapi2.yaml
│
├── deployments/                         # 📦 Общая инфраструктура
│   └── docker-compose.yml
│
├── tests/                               # 🧪 e2e / контрактные тесты
│   ├── e2e_flows/
|   |   ├── abstract_e2e1.go
│   |   └── abstract_e2e2.go
│   ├── integration/
│   |   └── abstract_services_bridge_test.go
│   └── health_checks/
|       ├── abstract_service1_check.go
│       └── abstract_service2_check.go
│
├── scripts/                             # 🛠️ CLI-утилиты, миграции и т.п.
│
├── go.work                              # 📦 go workspace
├── Makefile                             # 🧱 комманды
└── README.md
```


---

## 📐 Описание слоёв микросервиса

| Директория                             | Назначение                                                                                           |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| `cmd/`                                 | **Точка входа** в приложение (main.go). Подключение конфигов, логгера, DI, запуск сервера.           |
| `internal/app/`                        | **Бизнес-логика**: use-case'ы, сценарии, координация вызовов репозиториев и сервисов.                |
| `internal/domain/model/`               | **Чистые сущности**: структуры (User, Order…), без зависимостей от фреймворков.                      |
| `internal/domain/service/`             | Интерфейсы бизнес-сервисов, которые реализуются в `app/` (если нужно отделить бизнес-сервисы).       |
| `internal/domain/events/`              | **Domain-события**: структура событий, которые можно публиковать (UserCreated, PaymentFailed и т.д.) |
| `internal/domain/repository/`          | Интерфейсы репозиториев (например, `UserRepository`), без реализации.                                |
| `internal/infrastructure/cache/`       | Работа с Redis, Memcached и прочими кэшами.                                                          |
| `internal/infrastructure/externalapi/` | Обёртки над внешними API — Stripe, Telegram, сторонние сервисы.                                      |
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

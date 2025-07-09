# 🧱 Backend Python Microservices Boilerplate

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

backend-python/
├── services/                            # 💡 Все микросервисы
│   ├── abstract_service1/
│   │   ├── app/
│   │   │   ├── api/                     # 🔁 HTTP-роуты
│   │   │   │   ├── v1/			 # 📁 Версия api
│   │   │   │   │   ├── abstract_endpoints1.py
|   |   |   |   |   ├── abstract_endpoints2.py
│   │   │   │   │   └── __init__.py
│   │   │   │   └── __init__.py
│   │   │   ├── cmd/                     # 🚀 Точка входа в приложение
│   │   │   │   └── api/
│   │   │   │       └── main.py
│   │   │   ├── core/                    # ⚙️ Настройки, конфиги
│   │   │   │   ├── settings.py
|   |   |   |   └── __init__.py
│   │   │   ├── dependencies/            # 🔗 Depends-инъекции
│   │   │   │   ├── abstract_dependency1.py
│   │   │   │   ├── abstract_dependency2.py
│   │   │   │   └── __init__.py
│   │   │   ├── events/                  # 📡 Startup/shutdown хуки
│   │   │   │   ├── abstract_event1.py
│   │   │   │   ├── abstract_event2.py
│   │   │   │   └── __init__.py
│   │   │   ├── exceptions/              # 🚨 Кастомные исключения и хендлеры
│   │   │   │   ├── handlers/
│   │   │   │   |   ├── abstract_handler1.py
│   │   │   │   |   ├── abstract_handler2.py
│   │   │   │   |   └── __init__.py
│   │   │   │   ├── abstract_exception1.py
│   │   │   │   ├── abstract_exception2.py
│   │   │   │   └── __init__.py
│   │   │   ├── middlewares/             # 🧱 Middleware-логика
│   │   │   │   ├── abstract_middleware1.py
│   │   │   │   ├── abstract_middleware1.py
│   │   │   │   └── __init__.py
│   │   │   ├── models/                  # 🗄️ ORM-модели (SQLAlchemy)
│   │   │   │   ├── abstract_model1.py
│   │   │   │   ├── abstract_model2.py
│   │   │   │   └── __init__.py
│   │   │   ├── repository/              # 💾 Работа с базой (CRUD)
│   │   │   │   ├── abstract_repo1.py
│   │   │   │   ├── abstract_repo2.py
│   │   │   │   └── __init__.py
│   │   │   ├── schemas/                 # 🧾 Pydantic-схемы
│   │   │   │   ├── abstract_scheme1.py
│   │   │   │   ├── abstract_scheme2.py
│   │   │   │   └── __init__.py
│   │   │   ├── services/                # 🧠 Бизнес-логика
│   │   │   │   ├── abstract_service1.py
│   │   │   │   ├── abstract_service2.py
│   │   │   │   └── __init__.py
│   │   │   └── utils/                   # 🛠️ Вспомогательные утилиты
│   │   │   |   ├── abstract_utility1.py
│   │   │   |   ├── abstract_utility2.py
│   │   │   │   └── __init__.py
│   │   │   └── __init__.py   
│   │   ├── tests/                       # 🧪 Unit-тесты сервиса
│   │   │   ├── test_user_routes.py
│   │   │   └── test_user_service.py
│   │   ├── .env
│   │   ├── Dockerfile
│   │   ├── pyproject.toml
│   │   └── README.md
│   └── abstract_service2/
│       └── app/ ... (аналогично)
│
├── libs/                                # 📦 Общие библиотеки (shared)
│   ├── shared_schemas/                  # 🔁 Общие Pydantic-схемы
│   │   ├── shared_abstract_scheme1.py
│   │   ├── shared_abstract_scheme2.py
│   │   └── __init__.py
│   ├── shared_utils/                    # 🧰 Общие утилиты
│   │   ├── shared_abstract_utility1.py
│   │   ├── shared_abstract_utility2.py
│   │   └── __init__.py
│   └── shared_config/                   # ⚙️ Глобальные настройки и константы
│       ├── shared_abstract_cfg1.py
│       └── shared_abstract_cfg2.py
│
├── tests/                               # 🧪 Интеграционные / e2e тесты
│   ├── abstract_integration_test1.py    # e2e тест взаимодействия сервисов
│   └── abstract_integration_test2.py    # проверка статуса всех сервисов
│
├── docker-compose.yml                   # 🧩 Общая сборка всех сервисов
├── .gitignore
├── LICENSE
└── README.md                            # 📖 Документация проекта
```


---

## 📐 Описание слоёв микросервиса

| Директория      | Назначение                                                                 |
| --------------- | -------------------------------------------------------------------------- |
| `api/`          | HTTP-роутинг, валидация входных данных, публичные интерфейсы               |
| `cmd/`          | Точка входа в приложение (обычно `main.py`)                                |
| `core/`         | Конфигурации, переменные окружения, логгеры и базовая инфраструктура       |
| `dependencies/` | Зависимости (внедрение через `Depends()`), доступ к БД, авторизация и т.д. |
| `events/`       | Логика, выполняемая при запуске и завершении приложения                    |
| `exceptions/`   | Кастомные исключения и обработчики ошибок                                  |
| `middlewares/`  | Глобальные обработчики запросов/ответов (например, CORS, логгирование)     |
| `models/`       | ORM-модели SQLAlchemy, отображающие структуру таблиц в БД                  |
| `repository/`   | Работа с данными: CRUD, запросы к БД                                       |
| `schemas/`      | Pydantic-схемы: валидация входных/выходных данных                          |
| `services/`     | Бизнес-логика: сценарии, агрегирование репозиториев                        |
| `utils/`        | Вспомогательные функции, например: хеширование, генерация токенов          |
| `tests/`        | Unit-тесты логики сервиса                                                  |

Подробнее про слоистую архитектуру: [здесь](https://testengineer.ru/layered-architecture/)


---

## 📄 Лицензия

Проект распространяется под лицензией MIT. Используйте свободно.

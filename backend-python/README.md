# 🧱 Backend Python Boilerplate

[![MIT License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

Это шаблон для учебных backend-проектов на Python с попыткой приблизиться к продакшн-ready подходу, в котором содержится чёткое разделение слоёв и подготовленная архитектура.

---

## 🎯 Цель

Этот шаблон создан для:

- Командной разработки с единой архитектурой
- Отработки паттернов разработки (DI, repository, services)
- Обучения новичков хорошей архитектуре

---

## 🌳 Дерево шаблона

```bash
backend-python
├── app
│   ├── api                                 # Роутеры и обработчики запросов
│   │   ├── __init__.py                     # Версия API
│   │   └── v1
│   │       ├── abstract_enpoints1.py
│   │       ├── abstract_enpoints2.py
│   │       └── __init__.py
│   ├── cmd
│   │   ├── api
│   │   │   ├── __init__.py
│   │   │   └── main.py                     # Точка входа в приложение
│   │   └── __init__.py
│   ├── core                                # Конфигурация, настройки
│   │   ├── __init__.py
│   │   └── settings.py
│   ├── __init__.py
│   ├── models                              # ORM модели
│   │   ├── abstract_model1.py
│   │   ├── abstract_model2.py
│   │   └── __init__.py
│   ├── repository                          # Абстракции для работы с БД
│   │   ├── abstract_repo1.py
│   │   ├── abstract_repo2.py
│   │   └── __init__.py
│   └── services                            # Бизнес-логика
│       ├── abstract_service1.py
│       ├── abstract_service2.py
│       └── __init__.py
├── docker-compose.yml                      # Сборка и запуск всех сервисов
├── Dockerfile                              # Образ backend-приложения
├── LICENSE                                 # MIT или любая другая лицензия
├── pyproject.toml                          # Зависимости и настройки проекта
├── README.md                               # Ты читаешь это 👀
└── tests                                   # Тесты (pytest)
    ├── test_component1.py
    └── test_component2.py
```

---

## 📐 Описание слоев

| Слой         | Какая логика будет лежать          |
| ------------ | ---------------------------------- |
| `api`        | HTTP-интерфейс, роутинг, валидация |
| `services`   | Бизнес-логика, сценарии            |
| `repository` | Работа с базой данных              |
| `models`     | Структуры данных, SQLAlchemy       |
| `core`       | Конфигурация приложения            |
| `cmd`        | Точка входа для запуска            |

Подробнее про слоистую архитектуру: [здесь](https://testengineer.ru/layered-architecture/)

---

## 📄 Лицензия

Проект распространяется под лицензией MIT. Используйте свободно.

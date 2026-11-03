#  Правила ведения репозитория

Для проекта используются следующие основные ветки:

```text
main
develop
feature/*
fix/*
docs/*
```

## main

Содержит стабильную версию проекта.

## develop

Используется для объединения результатов разработки.

## feature/*

Используется для разработки новых функций.

Примеры:

```text
feature/client-auth
feature/transport-selection
feature/hotel-selection
feature/trip-creation
```

## fix/*

Используется для исправления дефектов.

Например:

```text
fix/distance-service-timeout
fix/password-hashing
```

## docs/*

Используется при значительных изменениях документации.

Например:

```text
docs/project-artifacts
```

---

 Правила коммитов

Один коммит должен содержать одно логически завершённое изменение.

Используются следующие обозначения:

```text
feat: новая функциональность
fix: исправление
docs: документация
test: тесты
refactor: изменение структуры кода
chore: техническое изменение
```

Примеры:

```text
docs: add project scope
docs: add architecture diagram
docs: describe service contracts
feat: add client DTOs
feat: add distance calculation service
test: add transport pricing tests
```

Не допускаются малоинформативные сообщения:

```text
update
fix
work
123
изменения
```

---
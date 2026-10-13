# 7. Внутренние зависимости

Внутренние зависимости определяют взаимодействие проектов внутри решения.

```text
Wpf
 ↓
Services
 ↓
Data
 ↓
Domain

Services
 ↓
Infrastructure
 ↓
Domain

Contracts     ← используется Wpf, Services, Infrastructure
                (ни от кого не зависит)
```

При этом бизнес-модули не должны напрямую обращаться к внутренним классам друг друга.

Взаимодействие выполняется через публичные интерфейсы, зарегистрированные в DI.

Например, RentalModule получает данные об отеле для поездки:

```text
RentalModule
      ↓
IHotelRepository
      ↓
CatalogModule (реализация в Data)
```

Это позволяет уменьшить связанность модулей.

---

# 8. Внешние зависимости

На этапе проектирования предполагается использование следующих внешних компонентов.

| Зависимость | Версия | Назначение |
|---|---|---|
| .NET | 9.0 | Платформа выполнения |
| WPF | входит в .NET 9 | Клиентский интерфейс |
| Entity Framework Core | 9.0 | Доступ к данным |
| Npgsql.EntityFrameworkCore.PostgreSQL | 9.0 | Провайдер EF Core для PostgreSQL |
| PostgreSQL | 16+ | Хранение данных |
| CommunityToolkit.Mvvm | 8.3+ | MVVM без бойлерплейта |
| Microsoft.Extensions.Hosting | 9.0 | DI, конфигурация, логирование |
| MaterialDesignThemes | 5.x | Стили и темы WPF |
| BCrypt.Net-Next | 4.0+ | Хеширование паролей |
| Microsoft.Extensions.Http | 9.0 | IHttpClientFactory для внешних API |
| FluentValidation | 11.x | Валидация DTO |
| Nominatim (OpenStreetMap) | REST API | Внешний API геокодинга |
| OSRM | REST API | Внешний API маршрутизации |
| xUnit | 2.9+ | Модульное тестирование |
| Moq | 4.x | Мокирование в тестах |
| FluentAssertions | 6.x | Удобные assert'ы |
| Testcontainers.PostgreSql | 3.x | Интеграционные тесты на реальной БД |
| Git | 2.40+ | Контроль версий |

Конкретные версии библиотек фиксируются в файлах `.csproj`.

---
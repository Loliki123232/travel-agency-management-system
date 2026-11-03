# Курсовое проектирование. Этап 1  
## Разработка перечня артефактов и протоколов проекта

**Тема курсового проекта:** «Разработка системы управления туристическим агентством»

**МДК:** 02.02 «Инструментальные средства разработки программного обеспечения»  
**Стек:** C# / .NET 9, Git  
**Тип приложения:** веб-приложение с серверной частью на ASP.NET Core

---

# 1. Исходное состояние проекта

Перед началом выполнения этапа был создан Git-репозиторий курсового проекта.

Исходное состояние было проверено командой:

```bash
git status
```

Текущая ветка:

```text
main
```

Для выполнения этапа создана отдельная ветка:

```bash
git checkout -b Docs/project-artifacts
```

В рамках данного этапа выполняется проектирование структуры программной системы и подготовка проектной документации.

---

# 2. Назначение и границы проекта

## 2.1. Назначение системы

Разрабатываемая программная система предназначена для автоматизации деятельности турагентства.

Пользователь системы должен иметь возможность:

- зарегистрироваться и войти в систему;
- указать паспортные данные и контактную информацию;
- указать способ оплаты;
- выбрать тип транспорта (самолёт, поезд, автобус, корабль);
- указать города отправления и назначения;
- получить расчёт расстояния между городами;
- получить расчёт стоимости поездки на транспорте;
- выбрать отель по количеству звёзд;
- выбрать дополнительные услуги отеля (питание, удобства, экскурсии);
- оформить и сохранить поездку;
- просматривать историю своих поездок.

Администратор должен иметь возможность:

- войти в систему под учётной записью администратора;
- просматривать список клиентов, редактировать и удалять;
- просматривать список отелей, редактировать и удалять;
- просматривать все поездки;
- экспортировать данные в CSV и Excel.

---

## 2.2. Основные объекты предметной области

В системе выделены следующие основные сущности:

- клиент;
- способ оплаты;
- рейс (транспорт);
- отель;
- услуга отеля;
- поездка;
- администратор.

Типы транспорта:

- самолёт;
- поезд;
- автобус;
- корабль.

Категории услуг отеля: 

- питание;
- удобства;
- экскурсии.

---

## 2.3. Границы проекта

### В систему входит

1. Регистрация и аутентификация клиентов.
2. Аутентификация администраторов.
3. Ведение каталога отелей и их услуг.
4. Ведение списка рейсов по типам транспорта.
5. Расчёт расстояния между городами через внешний сервис.
6. Расчёт стоимости поездки на транспорте.
7. Расчёт стоимости проживания с учётом выбранных услуг.
8. Оформление поездки.
9. Просмотр истории поездок клиента.
10. Экспорт данных в CSV и Excel.
11. Хранение информации о клиентах.

### В систему не входит

В рамках курсового проекта не реализуются:

- реальные банковские платежи;
- интеграция с платёжными шлюзами;
- хранение CVV и полных номеров карт;
- интеграция с внешними системами бронирования;
- отправка SMS и email;
- мобильное приложение;
- онлайн-оплата.

Эти функции могут быть добавлены при дальнейшем развитии проекта.

---

# 3. Реестр проектных артефактов

Для проекта сформирован следующий перечень артефактов.

| № | Артефакт | Назначение | Расположение |
|---:|---|---|---|
| 1 | Описание назначения и границ проекта | Определяет функции и ограничения системы | `docs/project-artifacts/project-scope.md` |
| 2 | Перечень модулей | Описывает состав системы и ответственность модулей | `docs/project-artifacts/modules.md` |
| 3 | Архитектурная схема | Показывает структуру и связи модулей | `docs/project-artifacts/architecture.md` |
| 4 | Перечень зависимостей | Фиксирует внутренние и внешние зависимости | `docs/project-artifacts/dependencies.md` |
| 5 | Контракты взаимодействия | Определяет правила обмена между модулями | `docs/project-artifacts/contracts.md` |
| 6 | DTO | Определяет структуры передаваемых данных | `src/TourApp.Contracts/` |
| 7 | Инструкция сборки и запуска | Позволяет другому разработчику запустить проект | `README.md` |
| 8 | Тестовая стратегия | Определяет порядок проверки системы | `docs/project-artifacts/testing.md` |
| 9 | Правила работы с Git | Определяет правила ветвления и коммитов | `docs/project-artifacts/version-control.md` |
| 10 | Журнал технических решений | Фиксирует значимые архитектурные решения | `docs/decisions/` |

---

# 4. Перечень модулей системы

В проекте выделено четыре основных функциональных модуля.

| Модуль | Ответственность |
|---|---|
| `CustomerModule` | Регистрация, аутентификация, хранение данных клиентов и способов оплаты |
| `CatalogModule` | Хранение информации об отелях, услугах и рейсах |
| `RentalModule` | Создание и управление поездками, расчёт стоимости |
| `DeliveryModule` | Расчёт расстояния между городами через внешний сервис |

---

## 4.1. CustomerModule

Отвечает за работу с клиентами.

- Основные функции:
- регистрация клиента с хешированием пароля (BCrypt);
- аутентификация клиента и администратора;
- получение и изменение данных клиента;
- хранение способов оплаты (маскированный номер, без CVV);
- просмотр истории поездок;
- проверка уникальности email.

Пример роли пользователя:

```text
Client
Admin
```

Пример состояния способа оплаты:

```text
Unverified
Verified
Expired
```
---

## 4.2. CatalogModule

Отвечает за каталог отелей и рейсов.

Основные функции:

- получение списка отелей;
- получение информации об отеле с услугами;
- получение списка рейсов по типу транспорта;
- изменение данных отеля и рейса;
- управление услугами отеля (питание, удобства, экскурсии);
- фильтрация отелей по количеству звёзд.

Пример категорий услуг отеля:

```text
Meal
Amenity
Excursion
```

Пример типов транспорта:

```text
Plane
Train
Bus
Ship
```

Пример категорий услуг отеля:

```text
ThreeStars
FourStars
FiveStars
```

---

## 4.3. RentalModule

Отвечает за оформление и управление поездками.

Основные функции:

- создание черновика поездки;
- расчёт стоимости транспорта;
- расчёт стоимости отеля с услугами;
- подтверждение поездки;
- отмена поездки;
- просмотр поездок клиента;
- проверка доступности услуг.

Пример состояний поездки:

```text
Draft
Confirmed
Paid
Cancelled
```

Модуль взаимодействует с:

```text
CustomerModule
CatalogModule
DeliveryModule
```

---

## 4.4. DeliveryModule

Отвечает за расчёт расстояния между городами

Основные функции:

- геокодинг городов через Nominatim;
- построение маршрута через OSRM;
- возврат расстояния в километрах;
- кэширование результатов;
- троттлинг запросов (1 запрос в секунду для Nominatim).

Пример результата:

```text
Москва → Санкт-Петербург = 705 км
```

Пример состояний запроса к внешнему сервису:

```text
Success
CityNotFound
ServiceUnavailable
```

---

# 5. Архитектурная схема

Для проекта выбрана модульная архитектура.

```mermaid
flowchart TD
    WPF["TourApp.Wpf<br/>Views + ViewModels"]
    Services["TourApp.Services"]
    Customer["CustomerModule"]
    Catalog["CatalogModule"]
    Rental["RentalModule"]
    Infra["TourApp.Infrastructure<br/>DeliveryModule / DistanceService"]
    Nominatim["Nominatim API"]
    OSRM["OSRM API"]
    Data["TourApp.Data<br/>Repositories / TourDbContext"]
    PG[("PostgreSQL")]
    Domain["TourApp.Domain<br/>(сущности, enum'ы)"]
    Contracts["TourApp.Contracts<br/>(DTO, Requests)"]

    WPF -- "DI + DTO" --> Services
    Services --- Customer
    Services --- Catalog
    Services --- Rental
    Customer --> Rental
    Catalog --> Rental
    Rental -- "вызывает" --> Infra
    Infra --> Nominatim
    Infra --> OSRM
    Infra -- "Services → Data" --> Data
    Data --> PG

    Domain -. "используется всеми" .- WPF
    Domain -. "используется всеми" .- Services
    Domain -. "используется всеми" .- Data
    Domain -. "используется всеми" .- Infra

    Contracts -. "используется WPF" .- WPF
    Contracts -. "используется Services" .- Services
```

Центральным модулем бизнес-процесса является `RentalModule`.

При создании заказа он:

1. получает данные клиента из CustomerModule;
2. получает данные о рейсе (тип транспорта, города, дата) из CatalogModule;
3. получает данные об отеле и его услугах из CatalogModule;
4. рассчитывает расстояние между городами через DeliveryModule;
5. рассчитывает стоимость транспорта через ITransportPricingService;
6. рассчитывает стоимость отеля с учётом выбранных услуг через IHotelPricingService;
7. создаёт поездку и сохраняет её через ITripRepository в TourApp.Data.

---

# 6. Предполагаемая структура решения

```text
TourApp/
│
├── src/
│   │
│   ├── TourApp.Domain/          ← сущности (Model)
│   ├── TourApp.Contracts/       ← DTO, запросы, результаты, маппинг
│   ├── TourApp.Data/            ← EF Core, DbContext, репозитории
│   ├── TourApp.Services/        ← бизнес-логика
│   ├── TourApp.Infrastructure/  ← внешние API (Nominatim, OSRM)
│   └── TourApp.Wpf/             ← UI + ViewModels
│
├── tests/
│   │
│   ├── TourApp.UnitTests/
│   └── TourApp.IntegrationTests/
│
├── docs/
│   │
│   └── project-artifacts/
│       │
│       ├── project-scope.md
│       ├── modules.md
│       ├── architecture.md
│       ├── dependencies.md
│       ├── contracts.md
│       ├── testing.md
│       ├── version-control.md
│       │
│       └── decisions/
│           ├── ADR-001-layered-architecture.md
│           ├── ADR-002-mvvm.md
│           ├── ADR-003-ef-core.md
│           └── ADR-004-dto-layer.md
│
├── .gitignore
├── .editorconfig
├── README.md
└── TourApp.sln
```

---

# 7. Внутренние зависимости

Внутренние зависимости определяют взаимодействие проектов внутри решения.

```mermaid
flowchart TD
    Wpf["TourApp.Wpf"]
    Services["TourApp.Services"]
    Infra["TourApp.Infrastructure"]
    Data["TourApp.Data"]
    Domain["TourApp.Domain"]
    Contracts["TourApp.Contracts"]

    Wpf --> Services
    Services --> Data
    Services --> Infra
    Data --> Domain
    Infra --> Domain

    Wpf -. "используется" .-> Contracts
    Services -. "используется" .-> Contracts
    Infra -. "используется" .-> Contracts
```

При этом бизнес-модули не должны напрямую обращаться к внутренним классам друг друга.

Взаимодействие выполняется через публичные интерфейсы, зарегистрированные в DI.

Например, RentalModule получает данные об отеле для поездки:

```mermaid
flowchart TD
    Rental["RentalModule"]
    IHotelRepo["IHotelRepository<br/>(интерфейс)"]
    Catalog["CatalogModule<br/>(реализация в TourApp.Data)"]

    Rental --> IHotelRepo
    IHotelRepo --> Catalog
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

# 9. Основные DTO проекта

Для передачи данных между слоями и модулями создаются DTO.

## 9.1. ClientDto

```csharp
public record ClientDto(
    int Id,
    string Email,
    string FirstName,
    string LastName,
    string MiddleName,
    string Phone,
    bool IsAdmin
);
```

---

## 9.2. LoginRequest

```csharp
public record LoginRequest(
    string Email,
    string Password
);
```

---

## 9.3. RegisterRequest

```csharp
public record RegisterRequest(
    string Email,
    string Password
);
```

---

## 9.4. LoginResult

```csharp
public record LoginResult(
    bool Success,
    ClientDto? Client,
    string? ErrorCode
);
```

---

## 9.5. RegisterResult

```csharp
public record RegisterResult(
    bool Success,
    int? ClientId,
    string? ErrorCode
);
```

---

## 9.6. FlightDto

```csharp
public record FlightDto(
    Guid Id,
    string TransportType,
    string FromCity,
    string ToCity,
    decimal Price,
    DateTime DepartureTime
);
```

---

## 9.7. HotelDto

```csharp
public record HotelDto(
    Guid Id,
    string Name,
    int Stars,
    decimal BasePricePerNight,
    IReadOnlyList<HotelServiceDto> Services
);
```

---

## 9.8. HotelServiceDto

```csharp
public record HotelServiceDto(
    Guid Id,
    string Name,
    string Category,
    decimal Price
);
```

---

## 9.9. PaymentMethodDto

```csharp
public record PaymentMethodDto(
    int Id,
    string CardNumberMasked,
    string ExpiresAt
);
```

---

## 9.10. CreateTripRequest

```csharp
public record CreateTripRequest(
    int ClientId,
    string TransportType,
    string FromCity,
    string ToCity,
    DateTime DepartureDate,
    int Nights,
    Guid HotelId,
    IReadOnlyList<Guid> SelectedServiceIds
);
```

---

## 9.11. TripDto

```csharp
public record TripDto(
    Guid Id,
    int ClientId,
    string TransportType,
    string FromCity,
    string ToCity,
    decimal TransportPrice,
    decimal HotelPrice,
    decimal TotalPrice,
    DateTime DepartureDate,
    int Nights,
    string Status
);
```

---

## 9.12. CalculateDistanceRequest

```csharp
public record CalculateDistanceRequest(
    string FromCity,
    string ToCity
);
```

---

## 9.13. CalculateDistanceResult

```csharp
public record CalculateDistanceResult(
    bool Success,
    double? DistanceKm,
    string? ErrorCode
);
```

---

## 9.14. OperationResult

```csharp
public record OperationResult(
    bool Success,
    string? ErrorCode
);
```

---

## 9.15. UpdateClientRequest

```csharp
public record UpdateClientRequest(
    int Id,
    string FirstName,
    string LastName,
    string MiddleName,
    string Phone
);
```

---

## 9.16. UpdateHotelRequest

```csharp
public record UpdateHotelRequest(
    Guid Id,
    string Name,
    int Stars,
    decimal BasePricePerNight
);
```

---

# 10. Описание API

Клиентское приложение (WPF) не взаимодействует с системой через HTTP API. Серверная часть в проекте отсутствует: приложение настольное, все вызовы выполняются внутри процесса через интерфейсы, зарегистрированные в DI-контейнере.

Под «API» в проекте понимаются публичные интерфейсы слоёв `TourApp.Services` и `TourApp.Data`, а также внешние HTTP API (Nominatim и OSRM), которые вызывает `TourApp.Infrastructure`.

Предполагаются следующие основные точки взаимодействия.

| Метод | Интерфейс | Назначение |
|---|---|---|
| `LoginAsync` / `RegisterAsync` | `IAuthService` | Вход и регистрация клиента |
| `GetDistanceKmAsync` | `IDistanceService` | Расстояние между городами |
| `Calculate` | `ITransportPricingService` | Стоимость транспорта |
| `CalculateTotal` | `IHotelPricingService` | Стоимость отеля с услугами |
| `CreateAsync` / `CancelAsync` | `ITripService` | Создание и отмена поездки |
| `GetByEmailAsync` | `IClientRepository` | Получение клиента по email |
| `GetWithServicesAsync` | `IHotelRepository` | Отели с услугами |
| `FindAsync` | `IFlightRepository` | Поиск рейсов |
| `AddAsync` | `ITripRepository` | Сохранение поездки |

Внешние HTTP API, используемые проектом:

| Метод | Адрес | Назначение |
|---|---|---|
| `GET` | `https://nominatim.openstreetmap.org/search?q={city}&format=json` | Геокодинг города |
| `GET` | `https://router.project-osrm.org/route/v1/driving/{lon1},{lat1};{lon2},{lat2}?overview=false` | Расчёт маршрута |

---
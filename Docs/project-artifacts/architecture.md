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
7. создаёт поездку и сохраняет её через ITripRepository в TourApp.Data

---

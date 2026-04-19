# 🎬 TICinema — Microservices Cinema Ecosystem

Современная экосистема для управления кинотеатром, построенная на микросервисной архитектуре с использованием .NET 10, gRPC и RabbitMQ.

## 🏗 Архитектура (C4 Model)
Проект спроектирован с использованием методологии **C4 Model**. 
- **API Gateway:** Фасад для REST-клиентов.
- **gRPC Communication:** Высокопроизводительное взаимодействие между сервисами.
- **Event-Driven:** Асинхронные уведомления через RabbitMQ.

## 🛠 Стек технологий
- **Backend:** .NET 10, ASP.NET Core API.
- **Communication:** gRPC (Protobuf), MediatR (CQRS).
- **Database:** PostgreSQL (EF Core), Redis (Caching).
- **Messaging:** RabbitMQ (MassTransit).
- **Storage:** S3-compatible storage (Media Service).
- **Payments:** Stripe Integration.
- **DevOps:** Docker Compose, Prometheus, Grafana.

## 📦 Микросервисы
1. **Identity:** Auth, JWT, OTP (Mailtrap/Telegram).
2. **Movie & Theater:** Метаданные фильмов и топология залов.
3. **Screening:** «Мозг» системы — управление сеансами (с учетом UTC+5).
4. **Booking & Payment:** Бронирование мест и процессинг через Stripe.
5. **Notification:** Воркер для рассылок.
6. **Telegram Bot:** Интерфейс для быстрого взаимодействия.

## 🚀 Быстрый запуск
```bash
git clone [https://github.com/TICinema/TICinema.Infrastructure.git](https://github.com/TICinema/TICinema.Infrastructure.git)
cd docker
docker-compose up -d
```
graph TD
    User((Пользователь)) -->|REST| Gateway[API Gateway]
    
    %% Группировка через подграфы (визуально выделит бэкенд)
    subgraph "Core Microservices (gRPC)"
        Gateway -->|gRPC| Identity[Identity Service]
        Gateway -->|gRPC| Screening[Screening Service]
        Gateway -->|gRPC| Payment[Payment Service]
        
        Screening -->|gRPC| Movie[Movie Service]
        Screening -->|gRPC| Theater[Theater Service]
    end

    %% Внешние системы и события
    Payment -->|Webhook| Stripe{Stripe API}
    Payment -->|Events| RabbitMQ[RabbitMQ]
    RabbitMQ -->|Notify| Notification[Notification Service]

    %% Стилизация для красоты
    style Gateway fill:#f96,stroke:#333,stroke-width:2px
    style RabbitMQ fill:#ff6,stroke:#333
    style Stripe fill:#6772e5,color:#fff
```

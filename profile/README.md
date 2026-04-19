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

# 🎬 TICinema — Microservices Cinema Ecosystem

Современная экосистема для управления кинотеатром, построенная на микросервисной архитектуре с использованием .NET 10, gRPC и RabbitMQ.

## 🏗 Архитектура (C4 Model Level 2)

```mermaid
graph TD
    User((Пользователь)) -->|REST| Gateway[API Gateway]
    
    subgraph "Core Microservices (gRPC)"
        Gateway -->|gRPC| Identity[Identity Service]
        Gateway -->|gRPC| Screening[Screening Service]
        Gateway -->|gRPC| Payment[Payment Service]
        
        Screening -->|gRPC| Movie[Movie Service]
        Screening -->|gRPC| Theater[Theater Service]
    end

    Payment -->|Webhook| Stripe{Stripe API}
    Payment -->|Events| RabbitMQ[RabbitMQ]
    RabbitMQ -->|Notify| Notification[Notification Service]

    style Gateway fill:#f96,stroke:#333,stroke-width:2px
    style RabbitMQ fill:#ff6,stroke:#333
    style Stripe fill:#6772e5,color:#fff

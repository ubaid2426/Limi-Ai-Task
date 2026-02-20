AI Task Processing Microservices

A scalable Spring Boot microservices system that processes AI prompts asynchronously using RabbitMQ message queue and an external AI provider.

The system follows a Gateway → Queue → Worker architecture to ensure reliability, rate limiting, and background processing.

                ┌──────────────────────────┐
                │        Client App        │
                │ (Web / Mobile / Postman) │
                └────────────┬─────────────┘
                             │ HTTP Request
                             ▼
                ┌──────────────────────────┐
                │      Gateway Service     │
                │--------------------------│
                │ • Authentication (JWT)   │
                │ • Rate Limiting          │
                │ • Request Validation     │
                │ • Publish to Queue       │
                └────────────┬─────────────┘
                             │ RabbitMQ Message
                             ▼
                ┌──────────────────────────┐
                │        ActiveMQ          │
                │      Message Broker      │
                └────────────┬─────────────┘
                             │ Consumes Task
                             ▼
                ┌──────────────────────────┐
                │      Worker Service      │
                │--------------------------│
                │ • Calls External AI API  │
                │ • Processes Response     │
                │ • Stores AI Logs         │
                │ • Handles Failures       │
                └────────────┬─────────────┘
                             │
                             ▼
                ┌──────────────────────────┐
                │        Database          │
                │        (AI Logs)         │
                └──────────────────────────┘

Services Overview 
Gateway Service

Responsibilities

JWT Authentication

Rate limiting protection

Validates AI prompt requests

Sends tasks to ActiveMQ

Protects backend services

Key Components

AiController

JwtFilter

RateLimiterService

AiMessageProducer

SecurityConfig

Worker Service

Responsibilities

Consumes messages from RabbitMQ

Calls external AI provider

Processes AI response

Stores logs in database

Handles API failures gracefully

Key Components

AiRequestConsumer

ExternalAiClient

AiProcessingService

AiLogRepository

ai-task-system
│
├── gateway-service
│   ├── controller
│   ├── security
│   ├── messaging
│   ├── rateLimit
│   └── config
│
├── worker-service
│   ├── consumer
│   ├── ai
│   ├── service
│   ├── repository
│   └── entity
│
└── README.md

Flow

Gateway validates request

Message sent to RabbitMQ

Worker consumes message

Worker calls AI API

Result stored in database

login screen:
![img.png](img.png)

Ask Ai Screen:
![img_1.png](img_1.png)

Admin DashBoard Screen:
![img_2.png](img_2.png)

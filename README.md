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
<img width="1200" height="1000" alt="Screenshot 2026-02-20 at 9 31 18 PM" src="https://github.com/user-attachments/assets/64b66991-ef92-4088-923d-2531a6df6257" />


Ask Ai Screen:
<img width="1241" height="734" alt="Screenshot 2026-02-20 at 9 31 36 PM" src="https://github.com/user-attachments/assets/43481e6d-df2a-415b-acda-37cf8f6384ac" />


Admin DashBoard Screen:
<img width="1171" height="795" alt="Screenshot 2026-02-20 at 9 36 27 PM" src="https://github.com/user-attachments/assets/999db121-cdfd-4ede-b840-c575523ebce5" />


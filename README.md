# Scalable URL Shortening Platform

A high-performance, distributed URL shortening service implemented as a collection of independent microservices. The platform is designed for **high traffic**, **fast redirection**, and **real-time analytics**.

## System Design

<img width="1585" height="1350" alt="Untitled-2025-10-13-1211 (2)" src="https://github.com/user-attachments/assets/5f758fbb-2878-4221-a1b4-e7fa090ecd69" />



---

## Key Features

- **Microservices Architecture**  
  Three-service microservices architecture using Java Spring Boot and Node.js/TypeScript, logically separating URL management, ID generation, and analytics processing.

- **Distributed ID Generation**  
  Dedicated Ticket Service ensures unique, sequential short ID generation using **sharded PostgreSQL** with transactional row locking (`FOR UPDATE`) for high-throughput concurrency.

- **Performance Optimization**

  - **Redis caching** for near-instant URL lookups.
  - **Kafka** as an asynchronous message broker for non-blocking click event logging.

- **Data Pipeline & Security**
  - Clickstream events processed asynchronously and stored in **ClickHouse** for real-time analytics.
  - User authentication secured via **Spring Security** and **BCrypt hashing**.

---

## 🛠️ Technology Stack

| Service           | Primary Stack                | Database / Caching / Messaging                             |
| ----------------- | ---------------------------- | ---------------------------------------------------------- |
| Auth Service      | Spring Boot (Java)           | PostgreSQL (JPA/Flyway), Redis (Caching),Spring Security   |
| URL Service       | Spring Boot (Java)           | PostgreSQL (JPA/Flyway), Redis (Caching), Kafka (Producer) |
| Ticket Service    | Node.js (TypeScript/Express) | PostgreSQL (Sharded/Transactional)                         |
| Analytics Service | Node.js (TypeScript/Express) | ClickHouse (Analytics Store)                               |

---

## 📂 Project Structure

The project is divided into three main service directories, each with its own configuration and source code:

- **[Auth Service Repository (Spring Boot)](https://github.com/krishnadobhal/URL-Service_test)**
- **[URL Service Repository (Spring Boot)](https://github.com/krishnadobhal/URL-Service_test)**
- **[Ticket Service Repository (Node.js/TypeScript)](https://github.com/krishnadobhal/Ticket_Service)**
- **[Analytics Service Repository (Node.js/TypeScript)](https://github.com/krishnadobhal/Analytics_Service)**

---

## Setup & Running

Each service should be cloned and configured separately.

### Prerequisites

- Java 17+ (for URL Service)
- Node.js 18+ (for Ticket/Analytics Services)
- Docker (recommended for Redis, Kafka, PostgreSQL, and ClickHouse)

### Key Configurations

1. **Auth Service**

   - Set JWT Secret, database, Redis, and Kafka connection properties in `application.properties`.

2. **URL Service**

   - Configure `RandomIDURL` property to connect to Ticket Service.
   - Set database, Redis, and Kafka connection properties in `application.properties`.

3. **Ticket Service**

   - Requires environment variables for sharded PostgreSQL connections:
     ```bash
     SHARD_0=<DB_URL_0>
     SHARD_1=<DB_URL_1>
     ...
     ```
   - Additional environment variables for Redis/Kafka if required.

4. **Analytics Service**
   - Configure ClickHouse connection string in `.env` or service config.
   - Consume Kafka clickstream events asynchronously for analytics.

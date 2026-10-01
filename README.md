# Digital Banking Microservices Architecture

## Overview
This project is a scalable, distributed Digital Banking backend system built using a microservices architecture. It simulates core banking operations such as account management, secure transaction processing, third-party payment gateway integration, and real-time fraud detection. The system is designed with high availability and asynchronous event-driven communication in mind.

## Architecture & Technology Stack
- **Core Framework:** Java 17, Spring Boot 3.2
- **Inter-service Communication:** REST APIs (Synchronous), Apache Kafka (Asynchronous/Event-Driven)
- **Database:** MySQL (Spring Data JPA / Hibernate)
- **Caching:** Redis
- **Infrastructure & Containerization:** Docker, Docker Compose
- **Third-Party Integration:** Razorpay API (for wallet loading and external payments)

## Microservices Breakdown
The system is divided into decoupled services, each responsible for a specific business domain:
1. **API Gateway (Port 8080):** Acts as a single entry point for client requests, routing them to the appropriate backend services.
2. **Account Service (Port 8081):** Manages user account lifecycle, profiles, and balances. Maintains its own isolated MySQL database schema.
3. **Transaction Service (Port 8082):** Handles fund transfers, deposits, and withdrawals. Ensures ACID properties during transactions and publishes events to Kafka upon transaction completion.
4. **Payment Service (Port 8083):** Integrates with external payment gateways (Razorpay) to allow users to add funds to their accounts securely.
5. **Fraud Detection Service (Port 8084):** An event-driven service that consumes transaction data from Kafka in real-time to monitor and flag suspicious activities based on predefined rules.
6. **Notification Service (Port 8085):** Listens to Kafka topics and dispatches asynchronous alerts (email/SMS) to users upon successful transactions or security alerts.

## Key Learnings & Engineering Decisions
Building this project provided deep insights into designing and maintaining distributed systems. Key takeaways include:

- **Microservices Design Patterns:** Learned to decompose a monolithic architecture into manageable services, defining strict domain boundaries and maintaining database-per-service isolation to prevent tight coupling.
- **Event-Driven Architecture:** Transitioned from blocking HTTP calls to asynchronous messaging using Apache Kafka. This significantly improved the system's throughput, ensuring that services like Notifications and Fraud Detection do not bottleneck core transaction processing.
- **Data Caching Strategies:** Implemented Redis caching to reduce database latency for high-read operations (e.g., balance inquiries), optimizing overall system performance.
- **Handling Distributed Data:** Addressed the challenges of distributed transactions and data consistency. Learned how to manage inter-service communication securely and effectively using REST clients and event streams.
- **Infrastructure as Code:** Leveraged Docker and Docker Compose to orchestrate multiple services and infrastructure dependencies (MySQL, Redis, Zookeeper, Kafka) seamlessly, ensuring environment consistency from development to deployment.

## Getting Started

### Prerequisites
- Java 17 or higher
- Maven 3.8+
- Docker and Docker Compose

### Running the Application Locally
1. Start the infrastructure (MySQL, Redis, Kafka, Zookeeper) using Docker Compose:
   ```bash
   docker-compose up -d
   ```
2. Build the microservices using Maven:
   ```bash
   mvn clean install
   ```
3. Run each service individually or deploy them as containers depending on your environment setup.

## Future Enhancements
- Implementation of an API Gateway rate limiter to prevent DDoS attacks.
- Integration of Spring Cloud Config for centralized configuration management.
- Implementation of the Saga Pattern for better distributed transaction management.
- Adding comprehensive Unit and Integration tests using JUnit and Testcontainers.

# 🖥️ ComputerShop — Microservices E-commerce Platform

> A full-stack PC hardware store that lets customers discover components, build compatible PCs, and complete purchases through a gateway-based microservices backend.

## Overview

ComputerShop addresses the end-to-end online PC-buying workflow: product discovery, comparison, custom PC configuration, cart and order management, payments, and after-sales operations. It serves both shoppers and store staff through a customer storefront and an administrative dashboard. The backend was evolved from a single Spring Boot monolith into a **microservices architecture with an API Gateway and database-per-service boundaries**, while the remaining tightly coupled domains continue to run in the original application.

## Links

- Source code (Backend - Microservices): https://github.com/MSS301-Microservice/ComputerShop_BE
- Source code (Frontend): https://github.com/MSS301-Microservice/ComputerShop_FE
- Source code (Backend - Monolith): https://github.com/ComputerShop-vn/ComputerShop_BE

## 📸 Demo

[▶️ Watch video demo](https://youtu.be/C4sCJQyqjFY)

## Tech Stack

| Area | Technologies |
| --- | --- |
| Backend | Java 21, Spring Boot 3.5, Spring MVC, Spring Cloud Gateway (WebFlux), Spring Security, OAuth2 Resource Server, Spring Data JPA, Spring Mail, Spring WebSocket/STOMP, Springdoc OpenAPI, Maven |
| Frontend | React 19, TypeScript, Vite, React Router, Recharts, Motion, SockJS, STOMP.js |
| Database | Microsoft SQL Server |
| Integrations | Google Sign-In, Cloudinary, VNPay, SMTP email |
| DevOps / tooling | Docker, Docker Compose |

## Highlights

- **Gateway-based microservices migration:** decomposed the original backend into an API Gateway plus five extracted domain services—Identity, Catalog, Cart, PC Builder, and Order—while retaining the remaining domains in the monolith; the current topology contains **7 Spring Boot applications**.
- **Database-per-service ownership:** the extracted services own **five dedicated SQL Server databases**; the remnant monolith retains its own database, for **six databases** in the current architecture.
- **Secure cross-service design:** HS512 JWT authentication is validated by each service, while internal REST calls use a separate `X-Internal-Api-Key` boundary. The gateway intentionally does not expose `/internal/**` endpoints.
- **Custom PC builder:** stores builds and component selections, evaluates compatibility rules, and returns compatible component variants and filtering hints before checkout.
- **Order orchestration:** coordinates Cart, Catalog, Promotion, and Payment services; reserves inventory, snapshots product details into order items, supports COD and VNPay payment flows, including installment schedules.
- **Production-oriented commerce features:** Google sign-in, email OTP registration and password recovery, Cloudinary product-media management, real-time customer–staff chat via STOMP/WebSocket, reporting with Excel export, and pagination across administrative catalog/content workflows.

## Architecture

```text
React + Vite SPA
       |
       v
Spring Cloud API Gateway :8888
       |
       +-- user-service      :8081 --> ComputerShopUserDB
       +-- catalog-service   :8082 --> ComputerShopCatalogDB
       +-- cart-service      :8083 --> ComputerShopCartDB
       +-- pcbuilder-service :8084 --> ComputerShopPCBuildDB
       +-- order-service     :8085 --> ComputerShopOrderDB
       +-- remnant monolith  :8080 --> ComputerShopDB
```

Each backend application follows a layered **Controller → Service → Repository → Entity** structure. Services communicate synchronously over internal REST clients when data belongs to another domain; the API Gateway provides the single public entry point and attaches a correlation ID for request tracing.

The React client is organized around customer-facing flows (shop, product detail, cart, checkout, orders, warranty, PC builder, and comparison) and a dedicated administrative area for catalog, users/roles, orders, promotions, warranties, blogs, reporting, messages, and installment packages. DTO and MapStruct mapper layers keep API contracts separate from persistence models.

## My Role

**Primary Backend Developer — Microservices Migration & Commerce Modules**

- Independently transformed the phase-1 Spring Boot monolith into the current gateway-based microservices backend, defining service boundaries, separate SQL Server databases, routing, JWT validation, internal API-key protection, and REST-based service-to-service communication.
- In phase 1, Git history under `vinhhien88` shows my implementation of real-time customer–staff chat, email OTP verification and password recovery, authorization updates, CRUD modules for attributes/promotions/blogs, and pagination across the web application.
- Integrated and maintained cross-cutting commerce capabilities including VNPay payments/installments, Cloudinary media storage, Google sign-in, SMTP notifications, reporting exports, and the custom PC-build workflow.

## Project Scale

The current backend source contains:

- **7** Spring Boot applications: one API Gateway, five extracted domain services, and the remnant monolith
- **6** SQL Server databases in the current service topology
- **28** controller classes and **142** 142 REST API mappings
- **28** JPA entity classes
- **26** service interfaces with **26** corresponding service implementations

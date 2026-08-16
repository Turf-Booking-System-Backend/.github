# 🏟️ Turf Booking Platform

A **production-oriented multi-vendor Turf Booking Platform** built with **Java, Spring Boot, Microservices, MySQL, JPA/Hibernate, and JWT-based security**.

The platform allows users to discover and book turfs, vendors to manage their turfs, and administrators to control the overall platform.

---

## 🏗️ Microservices

The project currently contains **3 microservices**:

| Service              | Responsibility                  | Status            |
| -------------------- | ------------------------------- | ----------------- |
| 🔐 `auth-service`    | Authentication, JWT & roles     | ✅ Completed       |
| 🏟️ `vendor-service` | Vendors, turfs, slots & pricing | ✅ Core completed  |
| 📅 `booking-service` | Booking & cancellation          | ✅ Completed |

### Architecture

```text
                    ┌─────────────────┐
                    │   Client / UI   │
                    └────────┬────────┘
                             │
          ┌──────────────────┼──────────────────┐
          ▼                  ▼                  ▼
   ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
   │ auth-service│    │vendor-service│    │booking-service│
   └──────┬──────┘    └──────┬──────┘    └──────┬──────┘
          │                   │                  │
          ▼                   ▼                  ▼
      MySQL DB             MySQL DB           MySQL DB
```

Each service owns its **own database** and is developed independently.

---

# 🛠️ Technology Stack

### Backend

* **Java 17**
* **Spring Boot**
* **Spring Security**
* **JWT**
* **Spring Data JPA**
* **Hibernate ORM**
* **MySQL**
* **Maven**
* **Lombok**
* **Jakarta Validation**

### Development & Tools

* Docker
* Postman
* Git
* Bitbucket / GitHub
*  Cloudinary — Turf images storage

### Planned Technologies

* WebClient / OpenFeign — service-to-service communication
* React + Tailwind CSS — frontend

---

---

# 🔐 1. Auth Service

### Purpose

Provides the platform's authentication and authorization foundation.

### Main Features

* User registration
* User login
* BCrypt password encryption
* JWT generation & validation
* JWT filter
* Stateless authentication
* Role-based authorization
* `USER`, `VENDOR`, `ADMIN` roles
* `@PreAuthorize` method security
* Global exception handling
* Custom `401` / `403` responses
* Health API

### Roles

```text
USER
VENDOR
ADMIN
```

---

# 🏟️ 2. Vendor Service

### Purpose

Manages **vendors and everything related to their turfs**.

### Main Features

#### Vendor Management

* Vendor onboarding
* Vendor approval
* Vendor blocking
* Vendor ownership validation

#### Turf Management

* Turf creation
* Turf retrieval
* Turf update
* Turf soft deletion
* Vendor-owned turf protection

#### Turf Slots

* Slot creation
* Slot retrieval
* Slot update
* Slot deactivation
* Start/end timing
* Hourly pricing
* Slot validation

#### Availability

* Active/inactive turf
* Active/inactive slots
* Turf availability configuration

#### Images

Planned production design uses **Cloudinary** for image storage.

Only image metadata/URLs are stored in MySQL.

---

# 📅 3. Booking Service

### Purpose

Manages the **turf booking lifecycle**.

### Main Features

* Create booking
* Booking date validation
* Time validation
* Amount validation
* Prevent duplicate booking
* Booking status management
* Booking cancellation
* User booking history

### Current Booking Status

```text
PENDING
CONFIRMED
CANCELLED
```

### Double Booking Protection

A booking conflict is detected using:

```text
Turf + Slot + Booking Date
```

Existing `PENDING` or `CONFIRMED` bookings prevent another booking for the same slot/date.



# 🗄️ Database Architecture

Each microservice has an independent database:

```text
auth_service_db
vendor_service_db
booking_service_db
```

The services **do not share databases**.

MySQL is currently run using Docker.

Typical Docker mapping:

```text
3307:3306
```

```text
3307 → Host machine
3306 → MySQL inside Docker
```

---

# 🔒 Security Architecture

The current system uses **JWT-based authentication**.

The Auth Service generates JWTs containing authentication information such as the user's role.

Currently, there is **no API Gateway**.

Therefore, protected microservices will eventually validate JWTs themselves until a gateway/security boundary is introduced.

---

# 🚀 Development Approach

The project is being developed **incrementally**, rather than building everything at once.

```text
Design
  ↓
Implement
  ↓
Test API
  ↓
Validate database
  ↓
Exception handling
  ↓
Security
  ↓
Service integration
  ↓
Production hardening
```

Each service is developed and tested before moving to the next major service.

---

# 📈 Current Progress

```text
AUTH SERVICE
V1 Foundation & Authentication     ✅
V2 Authorization & Roles           ✅

VENDOR SERVICE
V3 Vendor & Turf Management        🟡
  Vendor onboarding                ✅
  Turf CRUD                        ✅
  Slots / timing / pricing         ✅
  Availability configuration       ✅
  Turf images stores at cloudinary ✅

BOOKING SERVICE
V4 Booking Management              🟡
  Service setup                    ✅
  Booking entity                   ✅
  Create booking                   ✅
  Validation                       ✅
  Double-booking prevention        ✅
  Cancellation                     ✅
  Booking history                  ✅
  JWT integration                  🔄
```

---

# 🎯 Planned Next furure 

### 💳 Payment Service

* Razorpay integration
* Order creation
* Payment verification
* Refund handling
* Payment status integration with Booking Service


---

# ⭐ Project Highlights
Project Also Available on BitBucket: https://bitbucket.org/faizans-workspace/workspace/projects/TRUF

This project demonstrates practical backend concepts including:
The project isdeveloped incrementally using feature branches and pull requests.

* Microservices architecture
* REST APIs
* JWT authentication
* Role-based authorization
* JPA/Hibernate
* Database-per-service architecture
* Vendor ownership enforcement
* Booking validation
* Double-booking prevention
* Soft deletion
* Exception handling
* Dockerized MySQL
* Service separation
* Incremental production-oriented development

---

## 📌 Project Goal

The goal is to build a **real-world, scalable Turf Booking Platform** while following industry practices rather than creating a simple CRUD application.

The project is intentionally developed step-by-step to understand **why each architectural decision is made**, how services communicate, and how the system evolves toward production.


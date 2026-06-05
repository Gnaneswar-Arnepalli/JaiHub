# JaiHub Database Schema

## Overview

This document defines the database structure for JaiHub MVP.

Database: PostgreSQL

ORM: Prisma

Total Tables:

1. users
2. vehicles
3. driver_status
4. rides
5. provider_quotes
6. ride_events
7. payments
8. ratings

---

# 1. users

Purpose:

Stores passengers, drivers, and admin accounts.

| Column        | Type         | Constraints   |
| ------------- | ------------ | ------------- |
| id            | UUID         | PK            |
| full_name     | VARCHAR(100) | NOT NULL      |
| email         | VARCHAR(255) | UNIQUE        |
| phone         | VARCHAR(15)  | UNIQUE        |
| password_hash | TEXT         | NOT NULL      |
| role          | ENUM         | NOT NULL      |
| profile_image | TEXT         | NULL          |
| is_verified   | BOOLEAN      | DEFAULT FALSE |
| created_at    | TIMESTAMP    | NOT NULL      |
| updated_at    | TIMESTAMP    | NOT NULL      |

Role Enum:

* PASSENGER
* DRIVER
* ADMIN

---

# 2. vehicles

Purpose:

Stores vehicle information associated with drivers.

| Column         | Type         | Constraints   |
| -------------- | ------------ | ------------- |
| id             | UUID         | PK            |
| driver_id      | UUID         | FK → users.id |
| vehicle_type   | ENUM         | NOT NULL      |
| vehicle_number | VARCHAR(30)  | UNIQUE        |
| vehicle_model  | VARCHAR(100) | NOT NULL      |
| vehicle_color  | VARCHAR(50)  | NOT NULL      |
| is_active      | BOOLEAN      | DEFAULT TRUE  |
| created_at     | TIMESTAMP    | NOT NULL      |

Vehicle Type Enum:

* BIKE
* AUTO
* CAB

---

# 3. driver_status

Purpose:

Stores live driver availability and location.

| Column       | Type          | Constraints   |
| ------------ | ------------- | ------------- |
| id           | UUID          | PK            |
| driver_id    | UUID          | FK → users.id |
| status       | ENUM          | NOT NULL      |
| current_lat  | DECIMAL(10,7) | NULL          |
| current_lng  | DECIMAL(10,7) | NULL          |
| last_seen_at | TIMESTAMP     | NOT NULL      |
| created_at   | TIMESTAMP     | NOT NULL      |
| updated_at   | TIMESTAMP     | NOT NULL      |

Driver Status Enum:

* AVAILABLE
* BUSY
* OFFLINE

---

# 4. rides

Purpose:

Stores ride requests and ride lifecycle information.

| Column                 | Type          | Constraints      |
| ---------------------- | ------------- | ---------------- |
| id                     | UUID          | PK               |
| passenger_id           | UUID          | FK → users.id    |
| driver_id              | UUID          | FK → users.id    |
| vehicle_id             | UUID          | FK → vehicles.id |
| provider_name          | ENUM          | NOT NULL         |
| pickup_lat             | DECIMAL(10,7) | NOT NULL         |
| pickup_lng             | DECIMAL(10,7) | NOT NULL         |
| destination_lat        | DECIMAL(10,7) | NOT NULL         |
| destination_lng        | DECIMAL(10,7) | NOT NULL         |
| estimated_distance_km  | DECIMAL(10,2) | NOT NULL         |
| estimated_duration_min | INTEGER       | NOT NULL         |
| final_fare             | DECIMAL(10,2) | NOT NULL         |
| status                 | ENUM          | NOT NULL         |
| requested_at           | TIMESTAMP     | NOT NULL         |
| accepted_at            | TIMESTAMP     | NULL             |
| started_at             | TIMESTAMP     | NULL             |
| completed_at           | TIMESTAMP     | NULL             |
| created_at             | TIMESTAMP     | NOT NULL         |

Provider Enum:

* UBER
* OLA
* RAPIDO

Ride Status Enum:

* REQUESTED
* SEARCHING
* MATCHED
* ACCEPTED
* ARRIVED
* STARTED
* COMPLETED
* CANCELLED
* EXPIRED

---

# 5. provider_quotes

Purpose:

Stores fare quotations returned by providers.

| Column            | Type          | Constraints   |
| ----------------- | ------------- | ------------- |
| id                | UUID          | PK            |
| ride_id           | UUID          | FK → rides.id |
| provider_name     | ENUM          | NOT NULL      |
| estimated_fare    | DECIMAL(10,2) | NOT NULL      |
| estimated_eta_min | INTEGER       | NOT NULL      |
| surge_multiplier  | DECIMAL(5,2)  | NOT NULL      |
| quote_expiry      | TIMESTAMP     | NOT NULL      |
| created_at        | TIMESTAMP     | NOT NULL      |

---

# 6. ride_events

Purpose:

Stores ride history and audit trail.

| Column     | Type      | Constraints   |
| ---------- | --------- | ------------- |
| id         | UUID      | PK            |
| ride_id    | UUID      | FK → rides.id |
| event_type | ENUM      | NOT NULL      |
| event_time | TIMESTAMP | NOT NULL      |
| metadata   | JSONB     | NULL          |

Event Enum:

* REQUESTED
* ACCEPTED
* ARRIVED
* STARTED
* COMPLETED
* CANCELLED

---

# 7. payments

Purpose:

Stores payment and commission information.

| Column            | Type          | Constraints   |
| ----------------- | ------------- | ------------- |
| id                | UUID          | PK            |
| ride_id           | UUID          | FK → rides.id |
| passenger_id      | UUID          | FK → users.id |
| total_amount      | DECIMAL(10,2) | NOT NULL      |
| provider_amount   | DECIMAL(10,2) | NOT NULL      |
| commission_amount | DECIMAL(10,2) | NOT NULL      |
| payment_status    | ENUM          | NOT NULL      |
| payment_method    | ENUM          | NOT NULL      |
| transaction_id    | VARCHAR(255)  | UNIQUE        |
| paid_at           | TIMESTAMP     | NULL          |

Payment Status Enum:

* PENDING
* SUCCESS
* FAILED
* REFUNDED

Payment Method Enum:

* UPI
* CARD
* NETBANKING
* WALLET

---

# 8. ratings

Purpose:

Stores passenger feedback for completed rides.

| Column       | Type      | Constraints   |
| ------------ | --------- | ------------- |
| id           | UUID      | PK            |
| ride_id      | UUID      | FK → rides.id |
| passenger_id | UUID      | FK → users.id |
| driver_id    | UUID      | FK → users.id |
| rating       | INTEGER   | CHECK (1-5)   |
| review       | TEXT      | NULL          |
| created_at   | TIMESTAMP | NOT NULL      |

---

# Redis Design

Redis will store:

* Live Driver Locations
* Driver Availability Cache
* Rate Limiting Data
* BullMQ Queues

Example:

drivers:hyderabad

Used with:

* GEOADD
* GEOSEARCH

---

# Future Enhancements

* Scheduled Rides
* AI Recommendation Engine
* Driver Earnings Dashboard
* Multi-City Support
* Surge Pricing Engine
* Kafka Event Streaming
* Kubernetes Deployment

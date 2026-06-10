# Security Design

## Purpose

This document defines the security architecture of RideHub.

Goals:

* Protect user data
* Prevent unauthorized access
* Prevent ride fraud
* Protect APIs against abuse
* Secure driver/passenger communication

---

# Authentication

Authentication uses JWT.

Flow:

1. User logs in.
2. Backend validates credentials.
3. Backend generates JWT token.
4. Client stores token.
5. Every API request sends:

Authorization: Bearer <token>

---

# User Roles

## Passenger

Permissions:

* Search rides
* Book rides
* Cancel rides
* View own rides

Cannot:

* Access driver APIs
* Access admin APIs

---

## Driver

Permissions:

* Accept rides
* Update location
* Complete rides
* View earnings

Cannot:

* Access admin APIs
* Access passenger data

---

## Admin

Permissions:

* View all rides
* View all users
* View platform analytics
* Manage disputes

---

# RBAC (Role Based Access Control)

Protected routes:

Passenger APIs:

/rides/search
/rides/book
/rides/history

Driver APIs:

/drivers/location
/drivers/status
/drivers/earnings

Admin APIs:

/admin/users
/admin/rides
/admin/analytics

---

# Password Security

Passwords never stored in plain text.

Algorithm:

bcrypt

Example:

password
↓

hashedPassword

Stored:

$2b$10$xxxxxxxxxxxxxxxx

---

# API Rate Limiting

Purpose:

Prevent:

* Fare scraping
* Brute-force attacks
* API abuse

Implementation:

Redis Sliding Window

Example:

Ride Search

Limit:

100 requests per minute

If exceeded:

HTTP 429

Too Many Requests

---

# Location Spoof Prevention

Problem:

Driver sends fake GPS coordinates.

Solution:

Validate:

* Speed consistency
* Route consistency
* Timestamp consistency

Flags:

IMPOSSIBLE_MOVEMENT

Example:

Driver Location:

Hyderabad

2 seconds later:

Mumbai

Result:

Flag Account

---

# HTTPS

All traffic encrypted using TLS.

Required for:

* Authentication
* Payments
* Ride tracking

---

# Payment Security

Payment provider handles:

* Card Details
* UPI Details

RideHub never stores:

* Card Number
* CVV
* UPI PIN

Only stores:

Payment ID
Transaction Status

---

# JWT Expiration

Access Token:

15 Minutes

Refresh Token:

7 Days

---

# Redis Security

Protected Data:

* Driver locations
* Active rides
* Rate limits

Redis not exposed publicly.

Accessible only through backend services.

---

# Audit Logging

Track:

* Login attempts
* Ride creation
* Ride cancellation
* Payment completion

Stored for investigation and analytics.

---

# Future Security Enhancements

* MFA
* Device Fingerprinting
* Fraud Detection AI
* Suspicious Ride Monitoring
* Geo-Fencing Validation

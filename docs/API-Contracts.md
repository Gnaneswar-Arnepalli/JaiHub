# JaiHub API Contracts

## Overview

This document defines all API endpoints required for the JaiHub MVP.

Base URL:

```http
/api/v1
```

Authentication:

```text
JWT Bearer Token
```

---

# Authentication APIs

## Register User

### Endpoint

```http
POST /auth/register
```

### Request

```json
{
  "fullName": "John Doe",
  "email": "john@example.com",
  "phone": "9876543210",
  "password": "Password@123",
  "role": "PASSENGER"
}
```

### Response

```json
{
  "success": true,
  "message": "User registered successfully"
}
```

---

## Login User

### Endpoint

```http
POST /auth/login
```

### Request

```json
{
  "email": "john@example.com",
  "password": "Password@123"
}
```

### Response

```json
{
  "accessToken": "jwt_token",
  "user": {
    "id": "uuid",
    "fullName": "John Doe",
    "role": "PASSENGER"
  }
}
```

---

# Ride APIs

## Search Ride

### Endpoint

```http
POST /rides/search
```

### Request

```json
{
  "pickupLat": 17.3850,
  "pickupLng": 78.4867,
  "destinationLat": 17.4500,
  "destinationLng": 78.3900,
  "vehicleType": "CAB"
}
```

### Response

```json
{
  "quotes": [
    {
      "provider": "UBER",
      "fare": 220,
      "eta": 8,
      "surgeMultiplier": 1.2
    },
    {
      "provider": "OLA",
      "fare": 240,
      "eta": 10,
      "surgeMultiplier": 1.3
    },
    {
      "provider": "RAPIDO",
      "fare": 210,
      "eta": 9,
      "surgeMultiplier": 1.1
    }
  ]
}
```

---

## Book Ride

### Endpoint

```http
POST /rides/book
```

### Request

```json
{
  "quoteId": "uuid"
}
```

### Response

```json
{
  "rideId": "uuid",
  "status": "SEARCHING"
}
```

---

## Get Ride Details

### Endpoint

```http
GET /rides/:rideId
```

### Response

```json
{
  "rideId": "uuid",
  "status": "ACCEPTED",
  "driver": {
    "id": "uuid",
    "name": "Ravi Kumar",
    "rating": 4.8
  },
  "vehicle": {
    "number": "TS09AB1234",
    "type": "CAB"
  }
}
```

---

## Cancel Ride

### Endpoint

```http
PATCH /rides/:rideId/cancel
```

### Response

```json
{
  "success": true,
  "message": "Ride cancelled successfully"
}
```

---

# Driver APIs

## Update Driver Status

### Endpoint

```http
PATCH /drivers/status
```

### Request

```json
{
  "status": "AVAILABLE"
}
```

### Response

```json
{
  "success": true
}
```

Driver Status:

* AVAILABLE
* BUSY
* OFFLINE

---

## Update Driver Location

### Endpoint

```http
POST /drivers/location
```

### Request

```json
{
  "latitude": 17.3850,
  "longitude": 78.4867
}
```

### Response

```json
{
  "success": true
}
```

---

# Payment APIs

## Get Payment Details

### Endpoint

```http
GET /payments/:rideId
```

### Response

```json
{
  "rideId": "uuid",
  "totalAmount": 220,
  "providerAmount": 200,
  "commissionAmount": 20,
  "paymentStatus": "SUCCESS"
}
```

---

# Health Check API

## Service Status

### Endpoint

```http
GET /health
```

### Response

```json
{
  "status": "UP"
}
```

---

# Error Response Format

All APIs must return errors in the following structure.

```json
{
  "success": false,
  "message": "Validation failed",
  "errors": [
    {
      "field": "email",
      "message": "Email is required"
    }
  ]
}
```

---

# Authentication Rules

Protected APIs require:

```http
Authorization: Bearer <token>
```

Protected Endpoints:

* Ride APIs
* Driver APIs
* Payment APIs

---

# API Versioning

Current Version:

```text
v1
```

Example:

```http
/api/v1/rides/search
```

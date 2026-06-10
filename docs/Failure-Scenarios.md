# Failure Scenarios Design

## Purpose

This document defines how RideHub handles failures and unexpected situations while maintaining system consistency and reliability.

---

# Scenario 1: Driver Accepts Ride But App Crashes

## Problem

Driver accepts ride.

Immediately after accepting:

* App crashes
* Phone shuts down
* Network disconnects

Passenger is waiting.

---

## Solution

Driver status remains:

BUSY

Ride status remains:

ACCEPTED

System waits:

5 Minutes

If no heartbeat received:

Driver marked OFFLINE

Ride automatically cancelled.

Passenger notified.

---

# Scenario 2: Driver Goes Offline During Ride

## Problem

Ride already started.

Driver loses internet connection.

---

## Solution

Ride continues.

Last known location stored.

System retries location updates.

When connection restored:

Location synchronization resumes.

Ride not cancelled.

---

# Scenario 3: Passenger Cancels During Driver Arrival

## Problem

Driver already travelling to pickup point.

Passenger cancels.

---

## Solution

Ride status:

CANCELLED_BY_PASSENGER

Driver returns:

AVAILABLE

Optional future enhancement:

Cancellation fee.

---

# Scenario 4: Payment Fails After Ride Completion

## Problem

Ride completed.

Payment gateway returns failure.

---

## Solution

Ride status:

COMPLETED

Payment status:

FAILED

Outstanding payment created.

Passenger can retry payment.

Driver earnings remain pending.

---

# Scenario 5: Double Driver Assignment

## Problem

Two drivers attempt to accept the same ride simultaneously.

---

## Solution

Database Transaction:

First successful transaction wins.

Ride status immediately updated:

ACCEPTED

Second request rejected.

Response:

409 Conflict

---

# Scenario 6: Redis Failure

## Problem

Redis unavailable.

Cannot perform:

* Driver matching
* Rate limiting
* Active driver lookup

---

## Solution

Fallback:

PostgreSQL Location Query

Performance degrades.

System remains operational.

Alert generated.

---

# Scenario 7: PostgreSQL Failure

## Problem

Database unavailable.

Cannot:

* Create rides
* Complete rides
* Process payments

---

## Solution

System enters:

READ_ONLY_MODE

Users informed.

Monitoring alerts triggered.

Database recovery initiated.

---

# Scenario 8: WebSocket Disconnect

## Problem

Passenger loses connection.

Cannot receive live tracking.

---

## Solution

Client reconnects automatically.

Socket rejoins ride room.

Latest ride state synchronized.

---

# Scenario 9: Third Party Provider Timeout

## Problem

Uber/Ola/Rapido simulator does not respond.

---

## Solution

Timeout:

3 Seconds

Provider ignored.

Remaining providers used.

Passenger still receives results.

---

# Scenario 10: Driver Location Spoofing

## Problem

Driver sends fake GPS coordinates.

---

## Solution

Validation Checks:

* Speed validation
* Route validation
* Timestamp validation

Suspicious accounts flagged.

Admin review required.

---

# Scenario 11: Duplicate Payment Request

## Problem

Passenger clicks payment button multiple times.

---

## Solution

Idempotency Key used.

Only one payment processed.

Additional requests ignored.

---

# Scenario 12: Queue Failure

## Problem

BullMQ job fails.

Example:

Commission calculation failed.

---

## Solution

Automatic retries:

3 attempts

If still failing:

Move to Dead Letter Queue (DLQ)

Admin notified.

---

# Scenario 13: Server Restart During Ride

## Problem

Backend service restarts.

Active rides exist.

---

## Solution

State stored in PostgreSQL.

Redis rebuilt during startup.

Users automatically reconnect.

No ride data lost.

---

# Scenario 14: Driver Reaches Wrong Location

## Problem

Driver claims arrival.

GPS indicates driver is far away.

---

## Solution

Arrival validation:

Within 100 meters of pickup point.

If validation fails:

Cannot mark ARRIVED.

---

# Scenario 15: Commission Recording Failure

## Problem

Ride completed.

Payment successful.

Commission save fails.

---

## Solution

Transactional Outbox Pattern

Transaction includes:

* Ride completion
* Payment update
* Commission event creation

Background worker retries until success.

Prevents financial inconsistency.

---

# Monitoring Alerts

Critical Alerts:

* PostgreSQL Down
* Redis Down
* Payment Failure Rate > 10%
* Queue Failure Rate > 5%
* Driver Matching Failure Rate > 5%

Alerts sent to:

* Admin Dashboard
* Email Notifications

---

# Reliability Principles

1. Never lose ride data.
2. Never lose payment records.
3. Never assign one ride to multiple drivers.
4. Every operation should be retryable.
5. System should degrade gracefully during failures.

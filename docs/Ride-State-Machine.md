# Ride State Machine

## Purpose

The Ride State Machine defines the valid lifecycle of a ride from creation to completion. It ensures that ride transitions are predictable, auditable, and prevent invalid state changes.

---

## Ride Lifecycle

```text
REQUESTED
    ↓
SEARCHING
    ↓
MATCHED
    ↓
ACCEPTED
    ↓
DRIVER_ARRIVING
    ↓
STARTED
    ↓
COMPLETED
```

---

## Ride States

### REQUESTED

Passenger submits ride request.

Conditions:

* Pickup Location
* Destination Location
* Vehicle Type Selected

---

### SEARCHING

RideHub searches available drivers.

Sources:

* Redis GEOSEARCH
* Driver Availability Service

---

### MATCHED

Suitable drivers found.

Factors:

* Distance
* Driver Rating
* ETA
* Traffic Conditions

---

### ACCEPTED

Driver accepts ride.

Actions:

* Driver state becomes BUSY
* Driver removed from available pool
* Passenger notified

---

### DRIVER_ARRIVING

Driver is travelling to pickup location.

Passenger receives:

* Live location
* ETA updates

---

### STARTED

Passenger enters vehicle.

Conditions:

* Driver confirms arrival
* Passenger confirms start

---

### COMPLETED

Ride successfully completed.

Actions:

* Payment enabled
* Commission calculated
* Driver earnings updated
* Ride archived

---

## Cancellation States

### Passenger Cancellation

Allowed Before:

* MATCHED
* ACCEPTED
* DRIVER_ARRIVING

Result:

```text
CANCELLED_BY_PASSENGER
```

---

### Driver Cancellation

Allowed Before:

* STARTED

Result:

```text
CANCELLED_BY_DRIVER
```

---

## Invalid State Transitions

Not Allowed:

```text
REQUESTED → COMPLETED
```

```text
MATCHED → COMPLETED
```

```text
STARTED → SEARCHING
```

```text
COMPLETED → STARTED
```

---

# Driver State Machine

```text
OFFLINE
    ↓
AVAILABLE
    ↓
BUSY
    ↓
AVAILABLE
```

---

### OFFLINE

Driver not accepting rides.

### AVAILABLE

Driver ready for matching.

### BUSY

Driver currently handling ride.

Cannot receive new ride requests.

---

# Payment State Machine

```text
PENDING
    ↓
SUCCESS
```

```text
PENDING
    ↓
FAILED
```

```text
SUCCESS
    ↓
REFUNDED
```

---

# Scheduled Ride Flow

```text
SCHEDULED
    ↓
SEARCHING
    ↓
MATCHED
    ↓
ACCEPTED
```

Ride search begins approximately 2 hours before scheduled departure.

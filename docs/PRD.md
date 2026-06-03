JaiHub PRD (Product Requirements Document)

1. Project Overview
Project Name

JaiHub

Tagline

One platform. Every ride. Best fare. Fair earnings.

Problem Statement

Passengers currently need to open multiple ride-hailing applications such as Uber, Ola, and Rapido to compare prices, waiting times, and ride availability.

Drivers also face challenges because ride requests are fragmented across multiple platforms, making it difficult to maximize earnings and reduce idle time.

JaiHub solves this by aggregating ride requests and ride availability into a single platform where both passengers and drivers can make optimal decisions.

2. Vision

Build India's smartest ride aggregation platform that benefits both passengers and drivers through transparent pricing, intelligent matching, and real-time decision making.


3. Target Users
Passenger

A user looking to book:

-Bike
-Auto
-Cab

with the best combination of:

-Price
-ETA(Estimated Time Of Arrival)
-Traffic Conditions
-Ride Quality


Driver:

A rider/driver who wants:

-More ride opportunities
-Better earnings
-Reduced idle time
-Fair ride allocation


4. Supported City (MVP)

Hyderabad

Future:

Bengaluru
Chennai
Mumbai
Delhi


5. Supported Vehicle Types

-Bike

Examples:

Rapido Bike

-Auto

Examples:

Rapido Auto
Ola Auto

-Cab

Examples:

Uber Go
Uber Premier
Ola Mini
Ola Prime



6. Revenue Model

JaiHub commission:

-10% per completed ride

Example:

Uber Fare = ₹200

JaiHub Fee = ₹20

Total = ₹220



7. Passenger Journey
Step 1

Passenger enters:

Pickup Location
Destination
Vehicle Type

#-------------

Step 2

JaiHub requests fares from:

Uber Simulator
Ola Simulator
Rapido Simulator

#-----------------

Step 3

JaiHub compares:

Fare
ETA
Driver Rating
Traffic Conditions
Distance

#-----------------------


Step 4

AI Recommendation Engine ranks rides.

Example:

Recommended:
Uber

Reason:
₹20 cheaper
4 minutes faster
Less traffic

#----------------

Step 5

Passenger books ride.
#--------------------------------

Step 6

Once booked:

All duplicate ride requests are removed.

No double booking.
#------------------------------

Step 7

Passenger receives:

Driver details
Vehicle details

Live location

#---------------------------------

Step 8

Ride starts.

#--------------------------------

Step 9

Ride completes.


#---------------------------------

Step 10

Payment gateway opens.

Payment occurs only after:

Ride Completed


#--------------------------------


8. Driver Journey

Step 1

Driver goes online.

Status:

AVAILABLE

#==============================
Step 2

Nearby rides appear.

#=============================

Step 3

Driver selects preferred ride.

Based on:

Distance
Fare
Earnings
Traffic

#===============================

Step 4

Driver accepts ride.

#================================

Step 5

Driver becomes:

BUSY

Across all providers.

#==============================

Step 6

Driver reaches passenger.

#==============================

Step 7

Ride starts.

#=============================

Step 8

Ride completes.

#=============================

Step 9

Driver becomes:

AVAILABLE

Again.

#==========================

9. Driver Availability Logic

Driver states:

AVAILABLE

BUSY

OFFLINE

#________________________________

Rules:

AVAILABLE
→ Can accept rides

BUSY
→ Cannot accept rides

OFFLINE
→ Hidden from searches

#__________________________________


10. Ride Acceptance Rules

Before passenger can accept ride:

Driver must be:

Within configured radius

Example:

Bike
15 minutes
Auto
20 minutes
Cab
30 minutes


11. Scheduled Rides

Passenger may book rides in advance.

Maximum scheduling window:

2 hours



12. AI Features (Phase 2)

Smart Ride Recommendation

Suggest best ride based on:

Fare
Traffic
ETA
Driver Rating
Demand Prediction


#-------------------------------

Predict:

Peak hours
Busy areas
Traffic Prediction


#--------------------------------

Estimate:

Congestion
Route delays
Driver Suggestions

#---------------------------------

Recommend:

High demand zones
Better earning areas

#----------------------------------





13. Traffic Intelligence

Inputs:

Driver GPS
Passenger GPS
Historical ride data

#-----------------------------

Outputs:

Faster route
Cheaper route
Lower traffic route

#----------------------------



14. Payment Flow

Ride Completion

↓

Payment Gateway Opens

↓

User Pays

↓

Commission Calculated

↓

Driver Earnings Recorded

↓

Ride Closed



15. Success Metrics

Passenger Metrics:

Ride completion rate
Average booking time
Average wait time


Driver Metrics:

Daily earnings
Acceptance rate
Idle time reduction


Business Metrics

Revenue
Active users
Daily rides



16. MVP Scope

Included:

✅ Authentication

✅ Driver Management

✅ Ride Search

✅ Fare Comparison

✅ Ride Booking

✅ Driver Matching

✅ Redis GEOSEARCH

✅ Live Tracking

✅ Ride Completion

✅ Payments




Technology Stack

Frontend:

Next.js
TypeScript
TailwindCSS

Backend:

NestJS
TypeScript

Database:

PostgreSQL
Prisma ORM

Caching:

Redis

Queue:

BullMQ

Realtime:

Socket.IO

Maps:

Mapbox

Containerization:

Docker

Deployment:

AWS (Future)
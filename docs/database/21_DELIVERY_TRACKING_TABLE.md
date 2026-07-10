# DELIVERY TRACKING TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The delivery_tracking table stores real-time location updates for riders during active deliveries.

It enables customers, riders, sellers, and administrators to monitor delivery progress while supporting route optimization, ETA estimation, delivery analytics, and future logistics improvements.

This table is optimized for high-frequency GPS updates without affecting the main deliveries table.

---

# Table Name

delivery_tracking

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Each tracking record represents one GPS location update generated during a delivery.

A single delivery may have thousands of tracking records depending on update frequency.

Tracking records are immutable and preserved for operational analytics.

---

# Ownership

Each tracking record belongs to:

One delivery

One rider

One location update

---

# Main Columns

id

tracking_uuid

delivery_id

rider_id

latitude

longitude

accuracy

speed

heading

altitude

tracking_time

tracking_source

status

created_at

---

# Tracking Sources

gps

network

manual

system

future_beacon

---

# Business Rules

Every tracking record belongs to one delivery.

Every tracking record belongs to one rider.

Latitude and longitude are mandatory.

Tracking records cannot be modified after creation.

Soft delete is not recommended.

---

# Usage

The tracking system supports:

Live customer tracking

Administrator monitoring

Rider route history

ETA calculation

Delivery verification

Operational analytics

Future route optimization

---

# Tracking Status

active

paused

completed

cancelled

signal_lost

# Column Definitions

---

## id

Type

BIGSERIAL

Primary Key

Not Null

---

## tracking_uuid

Type

UUID

Not Null

Unique

System-generated globally unique tracking identifier.

---

## delivery_id

Type

BIGINT

Foreign Key

References

deliveries(id)

Not Null

Indexed

Associated delivery.

---

## rider_id

Type

BIGINT

Foreign Key

References

riders(id)

Not Null

Indexed

Rider who generated the location update.

---

## latitude

Type

DECIMAL(10,7)

Not Null

GPS latitude.

---

## longitude

Type

DECIMAL(10,7)

Not Null

GPS longitude.

---

## accuracy

Type

DECIMAL(8,2)

Nullable

GPS accuracy in meters.

---

## speed

Type

DECIMAL(8,2)

Nullable

Current rider speed in kilometers per hour.

---

## heading

Type

DECIMAL(6,2)

Nullable

Direction of travel in degrees.

Example

0° = North

90° = East

180° = South

270° = West

---

## altitude

Type

DECIMAL(8,2)

Nullable

Altitude above sea level in meters.

---

## tracking_time

Type

TIMESTAMP

Not Null

Time when the GPS reading was recorded.

---

## tracking_source

Type

VARCHAR(20)

Default

gps

Allowed Values

gps

network

manual

system

future_beacon

---

## status

Type

VARCHAR(20)

Default

active

Allowed Values

active

paused

completed

cancelled

signal_lost

---

## created_at

TIMESTAMP

Default CURRENT_TIMESTAMP

---

# Foreign Keys

delivery_tracking.delivery_id

→ deliveries.id

delivery_tracking.rider_id

→ riders.id

---

# Constraints

Every tracking record must belong to one delivery.

Latitude must be between -90 and +90.

Longitude must be between -180 and +180.

Tracking time cannot be in the far future.

Completed deliveries should not receive new tracking updates.

Tracking UUID must be unique.

---

# Indexes

PRIMARY KEY (id)

UNIQUE INDEX (tracking_uuid)

INDEX (delivery_id)

INDEX (rider_id)

INDEX (tracking_time)

INDEX (status)

Composite Index

(delivery_id, tracking_time)

(rider_id, tracking_time)

---

# GPS Validation Rules

The platform validates:

Latitude range

Longitude range

GPS accuracy

Timestamp validity

Movement consistency

Invalid GPS coordinates are rejected and logged for investigation.

---

# ETA Calculation Logic

Estimated arrival time is calculated using:

Current rider location

Customer destination

Traffic conditions (Future)

Road distance

Average travel speed

Historical delivery performance

ETA is recalculated whenever a new tracking record is received.

---

# Security & Privacy

Only authorized users can access live tracking information.

Customers may view only tracking for their own active deliveries.

Historical tracking data is available only to authorized administrators.

GPS data must be transmitted securely using encrypted connections.

Location history should never be exposed publicly.

# Live Map Integration

The delivery tracking system integrates with map providers to display real-time rider movement.

Supported features include:

Current rider location

Customer delivery destination

Recommended route

Estimated arrival time (ETA)

Traffic-aware routing (Future)

The application should update the rider's position at configurable intervals while balancing battery usage and network performance.

---

# Geofencing

The platform may define virtual geographic boundaries around important locations.

Examples include:

Seller pickup location

Customer delivery address

Warehouse

Distribution hub

When a rider enters or exits a geofenced area:

Arrival events are recorded.

Notifications may be triggered.

Operational analytics are updated.

---

# Route Analytics

Historical tracking records support route analysis.

Examples include:

Average delivery distance

Average delivery time

Idle time

Route efficiency

Traffic delay analysis

High-demand delivery zones

Rider performance comparison

These insights help optimize delivery operations.

---

# Example Records

## Example 1

Tracking UUID

550e8400-e29b-41d4-a716-446655440333

Delivery ID

1025

Rider ID

18

Latitude

23.8103310

Longitude

90.4125210

Speed

28.50 km/h

Tracking Source

gps

Status

active

---

## Example 2

Tracking UUID

91c74120-1ab7-4f2d-85aa-2f4c7b8d8faa

Delivery ID

1108

Rider ID

24

Latitude

22.3568510

Longitude

91.7831820

Speed

15.20 km/h

Tracking Source

network

Status

active

---

# Future Expansion

Future versions may support:

Traffic-aware routing

Offline GPS synchronization

Automatic route optimization

Geofence notifications

Heat maps

Driver behavior analysis

Battery-aware tracking

Multi-stop delivery routes

AI-powered ETA prediction

Indoor positioning support

---

# Best Practices

Record location updates at configurable intervals.

Avoid excessive GPS polling to preserve battery life.

Store immutable tracking records for auditing.

Encrypt all transmitted location data.

Restrict access to live tracking information.

Archive historical tracking data for analytics.

Monitor GPS anomalies to detect fraudulent activity.

---

# Conclusion

The delivery_tracking table provides a scalable, secure, and analytics-ready location tracking foundation for HAT-BAZAR.

It supports:

Real-time rider tracking

ETA estimation

Route history

Geofencing

Operational analytics

Future AI-powered route optimization

This design enables transparent delivery tracking while protecting user privacy and supporting long-term logistics growth.

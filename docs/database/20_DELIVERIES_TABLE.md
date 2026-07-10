# DELIVERIES TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The deliveries table manages the complete delivery lifecycle for every order on the HAT-BAZAR platform.

It connects orders, riders, customers, and delivery events while providing accurate tracking, proof of delivery, and operational monitoring.

The table is designed to support standard deliveries, express deliveries, return pickups, and future logistics expansion.

---

# Table Name

deliveries

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Each delivery record represents one delivery assignment.

Normally one order has one delivery.

Future versions may support multiple delivery attempts or split deliveries.

Every delivery is assigned to one rider.

---

# Ownership

Each delivery belongs to:

One order

One rider

One customer

One delivery address

---

# Main Columns

id

delivery_uuid

order_id

rider_id

customer_id

delivery_address_id

delivery_type

delivery_status

pickup_time

delivery_time

estimated_delivery_time

delivery_charge

otp_code

proof_of_delivery

notes

created_at

updated_at

deleted_at

---

# Delivery Types

standard

express

same_day

scheduled

return_pickup

refund_pickup

exchange

---

# Business Rules

Every delivery belongs to one order.

Every delivery has one rider after assignment.

Soft delete must be supported.

OTP verification is required before successful delivery.

Proof of delivery may be required.

Cancelled orders cannot be delivered.

---

# Usage

The delivery system supports:

Order delivery

Express delivery

Scheduled delivery

Return pickup

Refund pickup

Exchange delivery

Future multi-stop delivery

---

# Delivery Status

pending

assigned

picked_up

in_transit

arrived

delivered

failed

cancelled

returned

# Column Definitions

---

## id

Type

BIGSERIAL

Primary Key

Not Null

---

## delivery_uuid

Type

UUID

Not Null

Unique

System-generated globally unique delivery identifier.

---

## order_id

Type

BIGINT

Foreign Key

References

orders(id)

Not Null

Indexed

Associated customer order.

---

## rider_id

Type

BIGINT

Foreign Key

References

riders(id)

Nullable

Assigned rider.

NULL until assignment.

---

## customer_id

Type

BIGINT

Foreign Key

References

users(id)

Not Null

Customer receiving the order.

---

## delivery_address_id

Type

BIGINT

Foreign Key

References

addresses(id)

Nullable

Reference to the customer's saved address.

Historical orders should also store an address snapshot inside the orders table.

---

## delivery_type

Type

VARCHAR(30)

Not Null

Allowed Values

standard

express

same_day

scheduled

return_pickup

refund_pickup

exchange

---

## delivery_status

Type

VARCHAR(20)

Default

pending

Allowed Values

pending

assigned

picked_up

in_transit

arrived

delivered

failed

cancelled

returned

---

## pickup_time

Type

TIMESTAMP

Nullable

Time when rider collects the package.

---

## delivery_time

Type

TIMESTAMP

Nullable

Actual successful delivery time.

---

## estimated_delivery_time

Type

TIMESTAMP

Nullable

Estimated arrival time shown to customers.

---

## delivery_charge

Type

DECIMAL(12,2)

Default

0.00

Delivery fee charged for this shipment.

---

## otp_code

Type

VARCHAR(10)

Nullable

One-time verification code for secure delivery.

---

## proof_of_delivery

Type

TEXT

Nullable

Reference to proof such as:

Photo URL

Customer signature

Delivery confirmation document

---

## notes

Type

TEXT

Nullable

Internal delivery notes.

---

## created_at

TIMESTAMP

Default CURRENT_TIMESTAMP

---

## updated_at

TIMESTAMP

Auto Updated

---

## deleted_at

TIMESTAMP

Nullable

Soft Delete

---

# Foreign Keys

deliveries.order_id

→ orders.id

deliveries.rider_id

→ riders.id

deliveries.customer_id

→ users.id

deliveries.delivery_address_id

→ addresses.id

---

# Constraints

Each order should have one active delivery record.

Delivery UUID must be unique.

Delivery charge cannot be negative.

OTP is required before marking delivery as completed (when OTP delivery is enabled).

Delivery time cannot be earlier than pickup time.

Cancelled deliveries cannot later become delivered.

Soft deleted deliveries remain available for audit.

---

# Indexes

PRIMARY KEY (id)

UNIQUE INDEX (delivery_uuid)

INDEX (order_id)

INDEX (rider_id)

INDEX (customer_id)

INDEX (delivery_status)

INDEX (pickup_time)

Composite Index

(rider_id, delivery_status)

(customer_id, delivery_status)

(order_id, delivery_status)

---

# Delivery Lifecycle

Order Created

↓

Waiting for Assignment

↓

Rider Assigned

↓

Package Picked Up

↓

In Transit

↓

Arrived

↓

OTP Verified

↓

Delivered

If delivery fails:

↓

Failed

↓

Reassignment or Return Process

---

# OTP Verification Logic

Before completing delivery:

The customer receives a one-time verification code.

The rider enters the OTP.

The system validates the code.

If valid:

Delivery is marked as delivered.

If invalid:

Delivery remains pending until successful verification or manual resolution.

---

# Security & Privacy

Only assigned riders may update delivery status.

Customers may view only their own deliveries.

Sensitive customer address information must never be exposed unnecessarily.

All delivery status updates should be logged for auditing.

Proof of delivery files should be stored securely with controlled access.

# Live GPS Tracking Integration

The delivery system supports future real-time GPS tracking.

Each delivery may be linked with live location updates including:

Current latitude

Current longitude

Last location update time

Estimated arrival time (ETA)

Delivery route

Future map visualization

The actual GPS history should be stored in a dedicated delivery_tracking table rather than inside the deliveries table.

---

# Rider Earnings Integration

Every completed delivery contributes to rider earnings.

The platform may calculate:

Base delivery fee

Distance bonus

Peak hour incentive

Weather incentive

Referral reward

Penalty deductions

Daily, weekly, monthly, and yearly earning reports can be generated from completed delivery records.

---

# Failed Delivery Handling

A delivery may fail for several reasons.

Examples include:

Customer unavailable

Incorrect address

OTP verification failed

Customer refused delivery

Rider vehicle problem

Weather conditions

Operational issue

Each failed delivery should trigger a predefined workflow:

Retry delivery

Assign another rider

Return package to seller

Start refund process (if applicable)

Generate operational logs for auditing

---

# Example Records

## Example 1

Delivery UUID

550e8400-e29b-41d4-a716-446655440222

Order ID

1025

Rider ID

18

Delivery Type

standard

Delivery Status

delivered

Delivery Charge

80.00

Pickup Time

2026-07-09 09:10:00

Delivery Time

2026-07-09 10:05:30

---

## Example 2

Delivery UUID

91c74120-1ab7-4f2d-85aa-2f4c7b8d8f55

Order ID

1108

Rider ID

24

Delivery Type

express

Delivery Status

in_transit

Estimated Delivery

2026-07-09 15:30:00

Delivery Charge

150.00

---

# Future Expansion

Future versions may support:

Live GPS tracking

Multiple delivery attempts

Split deliveries

Proof of pickup

Digital customer signature

AI-powered rider assignment

Route optimization

Traffic-aware ETA prediction

Locker delivery

Drone delivery

Autonomous delivery vehicles

---

# Best Practices

Always use UUID for public delivery references.

Store GPS history separately from delivery records.

Protect proof-of-delivery files using secure storage.

Never permanently delete production delivery records.

Log every delivery status change for auditing.

Assign deliveries only to verified riders.

Continuously monitor delivery performance and failure rates.

---

# Conclusion

The deliveries table provides a secure, scalable, and production-ready logistics foundation for HAT-BAZAR.

It supports:

Order delivery management

Rider assignment

OTP verification

Proof of delivery

Delivery status tracking

Rider earnings integration

Future live GPS tracking

This design ensures efficient, transparent, and reliable delivery operations while supporting future logistics expansion and maintaining long-term data integrity.

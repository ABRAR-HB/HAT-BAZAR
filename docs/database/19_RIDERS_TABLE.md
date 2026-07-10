# RIDERS TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The riders table stores information about delivery partners responsible for collecting and delivering orders across the HAT-BAZAR platform.

It supports rider identity, verification, availability, performance tracking, and future GPS-based delivery services.

---

# Table Name

riders

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Each rider profile belongs to one registered user.

A rider must complete identity verification before accepting delivery requests.

The platform may approve, suspend, or deactivate riders based on operational policies.

---

# Ownership

Each rider belongs to:

One user account

One primary service area

One preferred vehicle

A rider can complete many deliveries.

---

# Main Columns

id

user_id

rider_code

vehicle_type

vehicle_number

license_number

national_id

service_area

availability_status

verification_status

rating

total_deliveries

status

created_at

updated_at

deleted_at

---

# Vehicle Types

motorcycle

bicycle

car

van

walking

future_drone

---

# Business Rules

Each rider must have one user account.

Rider codes must be unique.

Soft delete must be supported.

Only verified riders can accept delivery requests.

Suspended riders cannot receive new assignments.

---

# Usage

The rider system supports:

Order pickup

Order delivery

Cash collection

Cash-on-delivery

Return pickup

Refund pickup

Future express delivery

---

# Verification Status

pending

verified

rejected

suspended

---

# Rider Status

active

inactive

busy

offline

blocked
# Column Definitions

---

## id

Type

BIGSERIAL

Primary Key

Not Null

---

## user_id

Type

BIGINT

Foreign Key

References

users(id)

Not Null

Unique

Each user can have only one rider profile.

---

## rider_code

Type

VARCHAR(30)

Not Null

Unique

System-generated rider identifier.

Example

RDR000125

---

## vehicle_type

Type

VARCHAR(30)

Not Null

Allowed Values

motorcycle

bicycle

car

van

walking

future_drone

---

## vehicle_number

Type

VARCHAR(50)

Nullable

Vehicle registration number.

---

## license_number

Type

VARCHAR(100)

Nullable

Driving license number.

Required for motorcycles, cars and vans.

---

## national_id

Type

VARCHAR(50)

Not Null

Government-issued identification number.

---

## service_area

Type

VARCHAR(255)

Not Null

Primary delivery zone assigned to the rider.

---

## availability_status

Type

VARCHAR(20)

Default

offline

Allowed Values

online

offline

busy

break

---

## verification_status

Type

VARCHAR(20)

Default

pending

Allowed Values

pending

verified

rejected

suspended

---

## rating

Type

DECIMAL(3,2)

Default

5.00

Average customer rating.

---

## total_deliveries

Type

INTEGER

Default

0

Completed delivery count.

---

## status

Type

VARCHAR(20)

Default

active

Allowed Values

active

inactive

blocked

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

riders.user_id

→ users.id

---

# Constraints

Each rider must belong to exactly one user.

Rider code must be unique.

National ID must be unique.

Rating must remain between 0.00 and 5.00.

Total deliveries cannot be negative.

Blocked riders cannot receive assignments.

Soft deleted riders remain available for audit purposes.

---

# Indexes

PRIMARY KEY (id)

UNIQUE INDEX (user_id)

UNIQUE INDEX (rider_code)

INDEX (verification_status)

INDEX (availability_status)

INDEX (status)

Composite Index

(service_area, availability_status)

(status, verification_status)

---

# Rider Availability Logic

Only riders with:

Status = active

Verification = verified

Availability = online

are eligible to receive delivery assignments.

If a rider accepts a delivery:

Availability automatically changes to:

busy

After completing the delivery:

Availability returns to:

online

unless the rider manually goes offline.

---

# Verification Rules

Before activation the platform verifies:

National ID

Driving license (if applicable)

Vehicle information

Phone number

Identity documents

Only verified riders may collect customer payments or deliver orders.

---

# Security & Privacy

Only authorized administrators can view sensitive rider documents.

Customers never see national ID or license information.

All rider APIs require authentication and authorization.

Personally identifiable information must be protected according to platform privacy policies.

# Order Assignment Integration

The rider system integrates directly with the order management service.

When an order is ready for delivery:

The platform searches for eligible riders based on:

Service area

Availability status

Verification status

Current workload

Future GPS proximity

Once assigned:

The order references the rider.

The rider status changes to busy.

After successful delivery:

The rider's completed delivery count increases.

The rider becomes available for new assignments.

---

# Earnings Integration

Each completed delivery contributes to rider earnings.

The platform may calculate:

Delivery fee

Distance bonus

Peak hour incentive

Rain bonus

Referral bonus

Penalty deductions

Daily, weekly, and monthly earnings can be generated from completed delivery records.

---

# GPS & Live Tracking

Future versions support real-time rider tracking.

Location updates may include:

Latitude

Longitude

Last updated time

Current speed (optional)

Delivery route

Estimated arrival time

Customers can view live delivery progress without accessing sensitive rider information.

---

# Example Records

## Example 1

Rider Code

RDR000125

User ID

45

Vehicle Type

motorcycle

Service Area

Dhaka North

Verification

verified

Availability

online

Rating

4.92

Completed Deliveries

1350

Status

active

---

## Example 2

Rider Code

RDR000208

User ID

63

Vehicle Type

bicycle

Service Area

Chattogram

Verification

verified

Availability

busy

Rating

4.78

Completed Deliveries

640

Status

active

---

# Future Expansion

Future versions may support:

Multi-city rider assignment

Shift scheduling

Route optimization

Live GPS tracking

Proof of delivery (photo/signature)

Emergency SOS button

In-app rider chat

AI-powered delivery assignment

Performance leaderboards

Electric vehicle support

Drone delivery integration

---

# Best Practices

Use soft delete instead of permanent deletion.

Verify rider identity before activation.

Protect personal documents with strict access control.

Assign orders only to verified and available riders.

Continuously monitor rider performance.

Record all assignment and status changes for auditing.

Optimize assignment algorithms to reduce delivery time.

---

# Conclusion

The riders table provides a secure, scalable, and production-ready foundation for delivery operations in HAT-BAZAR.

It supports:

Verified rider management

Delivery assignment

Availability tracking

Performance monitoring

Earnings integration

Future GPS tracking

AI-assisted dispatching

This design ensures efficient delivery operations while maintaining security, transparency, and long-term scalability across the marketplace ecosystem.

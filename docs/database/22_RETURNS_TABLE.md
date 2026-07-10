# RETURNS TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The returns table manages customer return requests after an order has been delivered.

It supports return requests, seller review, administrative approval, return pickup, product inspection, refund processing, and operational analytics.

The table is designed to provide a transparent and auditable return management process.

---

# Table Name

returns

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Each return record represents one customer return request.

A return is linked to one completed order.

A single order may have one or more return requests depending on platform policy.

Each return request follows a structured approval workflow before a refund or replacement is processed.

---

# Ownership

Each return belongs to:

One order

One customer

One shop

Optional rider pickup

Optional refund

---

# Main Columns

id

return_uuid

order_id

customer_id

shop_id

delivery_id

return_reason

customer_note

return_type

pickup_required

pickup_date

return_status

seller_decision

admin_decision

refund_id

created_at

updated_at

deleted_at

---

# Return Types

refund

replacement

exchange

repair

partial_return

---

# Business Rules

Returns can only be requested for eligible delivered orders.

Each return request must belong to one order.

Soft delete must be supported.

Every return request must include a reason.

Seller and administrator decisions must be recorded.

---

# Usage

The return system supports:

Product returns

Replacement requests

Exchange requests

Repair requests

Refund processing

Return pickup scheduling

Future automated approvals

---

# Return Status

requested

under_review

approved

rejected

pickup_scheduled

picked_up

inspection

completed

cancelled


# Column Definitions

---

## id

Type

BIGSERIAL

Primary Key

Not Null

---

## return_uuid

Type

UUID

Not Null

Unique

System-generated globally unique return identifier.

---

## order_id

Type

BIGINT

Foreign Key

References

orders(id)

Not Null

Associated order.

---

## customer_id

Type

BIGINT

Foreign Key

References

users(id)

Not Null

Customer requesting the return.

---

## shop_id

Type

BIGINT

Foreign Key

References

shops(id)

Not Null

Shop responsible for the returned product.

---

## delivery_id

Type

BIGINT

Foreign Key

References

deliveries(id)

Nullable

Associated delivery record.

---

## return_reason

Type

VARCHAR(100)

Not Null

Examples

Damaged Product

Wrong Item

Missing Parts

Not as Described

Quality Issue

Other

---

## customer_note

Type

TEXT

Nullable

Additional explanation provided by the customer.

---

## return_type

Type

VARCHAR(30)

Not Null

Allowed Values

refund

replacement

exchange

repair

partial_return

---

## pickup_required

Type

BOOLEAN

Default

TRUE

Indicates whether a rider pickup is required.

---

## pickup_date

Type

TIMESTAMP

Nullable

Scheduled pickup date and time.

---

## return_status

Type

VARCHAR(30)

Default

requested

Allowed Values

requested

under_review

approved

rejected

pickup_scheduled

picked_up

inspection

completed

cancelled

---

## seller_decision

Type

VARCHAR(20)

Nullable

Allowed Values

approved

rejected

pending

---

## admin_decision

Type

VARCHAR(20)

Nullable

Allowed Values

approved

rejected

pending

---

## refund_id

Type

BIGINT

Foreign Key

References

refunds(id)

Nullable

Associated refund record.

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

returns.order_id

→ orders.id

returns.customer_id

→ users.id

returns.shop_id

→ shops.id

returns.delivery_id

→ deliveries.id

returns.refund_id

→ refunds.id

---

# Constraints

Only delivered and eligible orders can be returned.

Return UUID must be unique.

Return reason is mandatory.

Pickup date cannot be earlier than the request date.

Completed returns cannot be modified.

Soft deleted returns remain available for auditing.

---

# Indexes

PRIMARY KEY (id)

UNIQUE INDEX (return_uuid)

INDEX (order_id)

INDEX (customer_id)

INDEX (shop_id)

INDEX (return_status)

INDEX (created_at)

Composite Index

(customer_id, return_status)

(shop_id, return_status)

(order_id, return_status)

---

# Return Workflow

Order Delivered

↓

Customer Creates Return Request

↓

Seller Review

↓

Admin Review (if required)

↓

Return Approved

↓

Pickup Scheduled

↓

Product Inspection

↓

Refund / Replacement / Exchange

↓

Return Completed

---

# Seller & Admin Approval Rules

The seller reviews the request first.

If the seller approves:

The platform may automatically continue the workflow.

If the seller rejects:

The customer may appeal.

The administrator has the final authority in disputed cases.

Every approval or rejection should be logged for auditing.

---

# Security & Privacy

Customers can access only their own return requests.

Sellers can access only returns related to their shops.

Administrative users have broader access for dispute resolution.

Customer notes and uploaded evidence must be stored securely.

Every status change should be recorded in the audit log.

# Refund Integration

The return system integrates directly with the refund service.

If a return is approved for a refund:

A refund record is created.

The payment method is verified.

The refund amount is calculated.

The refund is processed according to platform policy.

If wallet refunds are enabled, the amount may be credited directly to the customer's wallet.

All refund transactions remain fully auditable.

---

# Delivery & Pickup Integration

Approved returns may require rider pickup.

The delivery module supports:

Pickup assignment

Pickup scheduling

Pickup confirmation

Proof of pickup

Return package tracking

Warehouse delivery

The assigned rider follows a workflow similar to normal order delivery.

---

# Example Records

## Example 1

Return UUID

550e8400-e29b-41d4-a716-446655440444

Order ID

1025

Return Type

refund

Return Status

approved

Pickup Required

Yes

Seller Decision

approved

Admin Decision

approved

---

## Example 2

Return UUID

91c74120-1ab7-4f2d-85aa-2f4c7b8d8fcc

Order ID

1108

Return Type

replacement

Return Status

pickup_scheduled

Pickup Date

2026-07-12 11:00:00

Seller Decision

approved

Admin Decision

pending

---

# Analytics

Return data provides operational insights such as:

Return rate by shop

Return rate by product

Most common return reasons

Average refund processing time

Average pickup time

Seller approval rate

Customer return frequency

Fraud detection reports

These insights help improve product quality and customer satisfaction.

---

# Future Expansion

Future versions may support:

Photo and video evidence

AI-assisted return validation

Automatic approval for eligible products

Barcode and QR code verification

Warehouse inspection workflow

Partial refund automation

International returns

Advanced fraud detection

Customer self-service return labels

---

# Best Practices

Always validate return eligibility on the server.

Keep complete status history for every return request.

Protect customer-uploaded evidence using secure storage.

Never permanently delete production return records.

Integrate tightly with refund and delivery systems.

Monitor abnormal return patterns to detect fraud.

Maintain detailed audit logs for every approval and rejection.

---

# Conclusion

The returns table provides a secure, transparent, and scalable return management foundation for HAT-BAZAR.

It supports:

Return requests

Seller and administrator approval

Pickup scheduling

Refund integration

Replacement and exchange workflows

Operational analytics

Future AI-assisted return processing

This design ensures a fair and efficient post-order experience while maintaining accountability, customer trust, and long-term scalability.


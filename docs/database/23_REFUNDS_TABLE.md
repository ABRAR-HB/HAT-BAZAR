# REFUNDS TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The refunds table manages all refund transactions initiated by the HAT-BAZAR platform.

It supports full refunds, partial refunds, wallet credits, bank transfers, payment gateway reversals, and financial auditing.

The refund system ensures accurate financial reconciliation while maintaining complete transparency for customers, sellers, administrators, and accounting teams.

---

# Table Name

refunds

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Each refund record represents one refund transaction.

A refund is usually linked to one completed return request, although administrative refunds may also be created independently.

Every refund maintains complete financial history for auditing and reporting.

---

# Ownership

Each refund belongs to:

One order

One customer

One payment

Optional return request

---

# Main Columns

id

refund_uuid

order_id

payment_id

return_id

customer_id

refund_type

refund_method

refund_amount

refund_reason

refund_status

processed_by

processed_at

transaction_reference

notes

created_at

updated_at

deleted_at

---

# Refund Types

full

partial

manual

goodwill

adjustment

---

# Refund Methods

original_payment

wallet

bank_transfer

mobile_banking

cash

future_store_credit

---

# Business Rules

Every refund belongs to one order.

Refund amount cannot exceed the original payment.

Refunds support both full and partial amounts.

Soft delete must be supported.

Every refund must have a valid processing status.

---

# Usage

The refund system supports:

Order cancellation refunds

Return refunds

Partial refunds

Wallet refunds

Administrative refunds

Payment correction

Future automated refunds

---

# Refund Status

pending

approved

processing

completed

failed

cancelled

reversed


# Column Definitions

---

## id

Type

BIGSERIAL

Primary Key

Not Null

---

## refund_uuid

Type

UUID

Not Null

Unique

System-generated globally unique refund identifier.

---

## order_id

Type

BIGINT

Foreign Key

References

orders(id)

Not Null

Associated customer order.

---

## payment_id

Type

BIGINT

Foreign Key

References

payments(id)

Not Null

Original payment associated with this refund.

---

## return_id

Type

BIGINT

Foreign Key

References

returns(id)

Nullable

Associated return request.

Administrative refunds may not require a return request.

---

## customer_id

Type

BIGINT

Foreign Key

References

users(id)

Not Null

Customer receiving the refund.

---

## refund_type

Type

VARCHAR(20)

Not Null

Allowed Values

full

partial

manual

goodwill

adjustment

---

## refund_method

Type

VARCHAR(30)

Not Null

Allowed Values

original_payment

wallet

bank_transfer

mobile_banking

cash

future_store_credit

---

## refund_amount

Type

DECIMAL(12,2)

Not Null

Refund amount approved for processing.

---

## refund_reason

Type

VARCHAR(255)

Not Null

Reason for the refund.

Examples

Order Cancelled

Returned Product

Payment Error

Customer Compensation

Administrative Adjustment

---

## refund_status

Type

VARCHAR(20)

Default

pending

Allowed Values

pending

approved

processing

completed

failed

cancelled

reversed

---

## processed_by

Type

BIGINT

Foreign Key

References

users(id)

Nullable

Administrator who approved or processed the refund.

---

## processed_at

Type

TIMESTAMP

Nullable

Time when the refund was completed.

---

## transaction_reference

Type

VARCHAR(255)

Nullable

Reference returned by the payment gateway, bank, or wallet system.

---

## notes

Type

TEXT

Nullable

Administrative notes regarding the refund.

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

refunds.order_id

→ orders.id

refunds.payment_id

→ payments.id

refunds.return_id

→ returns.id

refunds.customer_id

→ users.id

refunds.processed_by

→ users.id

---

# Constraints

Refund UUID must be unique.

Refund amount must be greater than zero.

Refund amount cannot exceed the original payment amount.

Processed refunds cannot be modified except through authorized reversal procedures.

Refund status must follow the approved workflow.

Soft deleted refund records remain available for financial audits.

---

# Indexes

PRIMARY KEY (id)

UNIQUE INDEX (refund_uuid)

INDEX (order_id)

INDEX (payment_id)

INDEX (customer_id)

INDEX (refund_status)

INDEX (processed_at)

Composite Index

(customer_id, refund_status)

(order_id, refund_status)

(payment_id, refund_status)

---

# Refund Processing Workflow

Refund Request

↓

Validation

↓

Approval

↓

Payment Gateway / Wallet Processing

↓

Completed

If processing fails:

↓

Failed

↓

Retry or Manual Review

If necessary:

↓

Reversed

---

# Financial Validation Rules

Before processing a refund the system verifies:

Original payment exists.

Payment has been completed.

Refund amount is valid.

Duplicate refunds are prevented.

Customer eligibility is verified.

Accounting records remain balanced.

---

# Security & Audit Requirements

Only authorized administrators may approve refunds.

Every refund action must be logged.

Financial records must never be permanently deleted.

Transaction references must be stored securely.

All refund-related APIs require authentication and authorization.


# Wallet Integration

The refund system integrates directly with the HAT-BAZAR wallet service.

When the selected refund method is wallet:

The approved refund amount is credited to the customer's wallet.

A corresponding wallet transaction record is created.

The customer's wallet balance is updated immediately.

Every wallet refund remains traceable through both the refunds and wallet_transactions tables.

---

# Payment Gateway Integration

Refunds may also be processed through external payment gateways.

Supported methods include:

Original card reversal

Mobile banking refund

Bank transfer

Digital payment gateway

Each successful refund stores:

Gateway transaction reference

Processing timestamp

Gateway response status

Failure reason (if applicable)

Gateway responses should always be logged for auditing and reconciliation.

---

# Example Records

## Example 1

Refund UUID

550e8400-e29b-41d4-a716-446655440555

Order ID

1025

Refund Type

full

Refund Method

wallet

Refund Amount

1250.00

Refund Status

completed

Processed By

Admin ID 3

---

## Example 2

Refund UUID

91c74120-1ab7-4f2d-85aa-2f4c7b8d8fdd

Order ID

1108

Refund Type

partial

Refund Method

original_payment

Refund Amount

350.00

Refund Status

processing

Gateway Reference

PG-REF-20260709-001

---

# Financial Reporting

Refund records support financial reporting such as:

Total refunds

Refunds by payment method

Refunds by shop

Refunds by customer

Average refund processing time

Refund success rate

Refund failure analysis

Accounting reconciliation

These reports assist finance, operations, and compliance teams.

---

# Future Expansion

Future versions may support:

Automatic refund approval

AI-assisted fraud detection

Multi-currency refunds

International payment refunds

Split payment refunds

Store credit management

Refund scheduling

Bulk refund processing

Advanced accounting integration

---

# Best Practices

Always validate refund eligibility before processing.

Prevent duplicate refund transactions.

Keep complete financial audit trails.

Store gateway responses securely.

Never permanently delete completed refund records.

Monitor unusual refund activity for fraud detection.

Reconcile refund records regularly with payment gateway reports.

---

# Conclusion

The refunds table provides a secure, transparent, and scalable financial refund infrastructure for HAT-BAZAR.

It supports:

Full and partial refunds

Wallet refunds

Payment gateway refunds

Financial auditing

Accounting reconciliation

Fraud prevention

Future automated refund workflows

This design ensures accurate financial operations while maintaining customer trust, regulatory compliance, and long-term scalability.

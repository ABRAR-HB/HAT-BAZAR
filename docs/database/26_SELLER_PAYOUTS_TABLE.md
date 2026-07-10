# SELLER PAYOUTS TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The seller_payouts table manages all payout transactions between the HAT-BAZAR platform and sellers.

It records seller earnings, commission deductions, platform fees, payout requests, payment processing, and settlement history while maintaining complete financial transparency and auditability.

---

# Table Name

seller_payouts

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Each payout record represents one settlement made to a seller.

A payout may include earnings from multiple completed orders within a settlement period.

The platform calculates seller earnings after deducting commissions, refunds, penalties, and applicable service fees.

---

# Ownership

Each payout belongs to:

One shop

One seller

Optional administrator approval

Optional settlement batch

---

# Main Columns

id

payout_uuid

shop_id

seller_id

settlement_period_start

settlement_period_end

gross_amount

commission_amount

service_fee

refund_deduction

penalty_deduction

net_payout

payout_method

payout_status

processed_by

processed_at

transaction_reference

created_at

updated_at

deleted_at

---

# Payout Methods

bank_transfer

mobile_banking

wallet

cash

future_crypto

---

# Business Rules

Each payout belongs to one shop.

Net payout cannot be negative.

Gross amount must be greater than or equal to all deductions.

Soft delete must be supported.

Every completed payout must have a transaction reference.

---

# Usage

The payout system supports:

Seller settlements

Commission deduction

Refund adjustment

Penalty deduction

Manual payout

Future automated settlements

---

# Payout Status

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

## payout_uuid

Type

UUID

Not Null

Unique

System-generated globally unique payout identifier.

---

## shop_id

Type

BIGINT

Foreign Key

References

shops(id)

Not Null

Associated seller shop.

---

## seller_id

Type

BIGINT

Foreign Key

References

users(id)

Not Null

Seller receiving the payout.

---

## settlement_period_start

Type

DATE

Not Null

Beginning of the payout settlement period.

---

## settlement_period_end

Type

DATE

Not Null

End of the payout settlement period.

---

## gross_amount

Type

DECIMAL(12,2)

Not Null

Total earnings before deductions.

---

## commission_amount

Type

DECIMAL(12,2)

Default

0.00

Platform commission deducted from seller earnings.

---

## service_fee

Type

DECIMAL(12,2)

Default

0.00

Additional platform service charges.

---

## refund_deduction

Type

DECIMAL(12,2)

Default

0.00

Amount deducted due to approved customer refunds.

---

## penalty_deduction

Type

DECIMAL(12,2)

Default

0.00

Operational penalties applied to the seller.

---

## net_payout

Type

DECIMAL(12,2)

Not Null

Final payout amount after all deductions.

Formula:

Gross Amount

− Commission

− Service Fee

− Refund Deduction

− Penalty Deduction

---

## payout_method

Type

VARCHAR(30)

Not Null

Allowed Values

bank_transfer

mobile_banking

wallet

cash

future_crypto

---

## payout_status

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

Administrator who processed the payout.

---

## processed_at

Type

TIMESTAMP

Nullable

Time when the payout was completed.

---

## transaction_reference

Type

VARCHAR(255)

Nullable

Reference returned by the payment provider or bank.

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

seller_payouts.shop_id

→ shops.id

seller_payouts.seller_id

→ users.id

seller_payouts.processed_by

→ users.id

---

# Constraints

Payout UUID must be unique.

Gross amount cannot be negative.

Net payout cannot be negative.

Settlement end date cannot be earlier than settlement start date.

Completed payouts cannot be modified except through authorized reversal procedures.

Soft deleted payout records remain available for auditing.

---

# Indexes

PRIMARY KEY (id)

UNIQUE INDEX (payout_uuid)

INDEX (shop_id)

INDEX (seller_id)

INDEX (payout_status)

INDEX (processed_at)

Composite Index

(shop_id, payout_status)

(seller_id, payout_status)

(shop_id, settlement_period_end)

---

# Payout Calculation Rules

Before processing a payout the system calculates:

Gross earnings

Platform commission

Service fees

Refund deductions

Penalty deductions

Net payout amount

The calculated net payout must never be negative.

---

# Settlement Workflow

Settlement Period Ends

↓

Calculate Seller Earnings

↓

Apply Deductions

↓

Administrator Approval (if required)

↓

Payment Processing

↓

Settlement Completed

If payment fails:

↓

Retry

or

Manual Review

---

# Financial Security & Audit Requirements

Only authorized finance administrators may approve payouts.

Every payout action must be logged.

Transaction references must be stored securely.

Financial records must never be permanently deleted.

All payout-related APIs require authentication and authorization.


# Accounting Integration

The seller payout system integrates directly with the financial and accounting modules.

Every completed payout should generate corresponding accounting entries for:

Seller payable reduction

Cash or bank payment

Platform commission recognition

Service fee recognition

Refund adjustments

Financial reconciliation should remain fully traceable.

---

# Financial Reporting

Seller payout data supports financial reporting including:

Total payouts

Pending payouts

Completed payouts

Failed payouts

Seller earnings summary

Platform commission summary

Average payout processing time

Settlement history

Monthly payout reports

These reports assist finance, operations, and management teams.

---

# Example Records

## Example 1

Payout UUID

550e8400-e29b-41d4-a716-446655440888

Shop ID

12

Seller ID

45

Gross Amount

25000.00

Commission

2500.00

Service Fee

300.00

Refund Deduction

500.00

Penalty

0.00

Net Payout

21700.00

Method

bank_transfer

Status

completed

---

## Example 2

Payout UUID

91c74120-1ab7-4f2d-85aa-2f4c7b8d8fbb

Shop ID

18

Seller ID

67

Gross Amount

18000.00

Commission

1800.00

Service Fee

200.00

Refund Deduction

0.00

Penalty

100.00

Net Payout

15900.00

Method

mobile_banking

Status

processing

---

# Future Expansion

Future versions may support:

Automatic weekly settlements

Daily payouts

Scheduled payouts

Multiple payout accounts

International bank transfers

Multi-currency settlements

Tax deduction automation

AI-based payout anomaly detection

ERP integration

Advanced financial reconciliation

---

# Best Practices

Validate payout calculations before approval.

Automate settlement calculations where possible.

Maintain complete audit logs.

Store transaction references securely.

Never permanently delete completed payout records.

Regularly reconcile payout records with bank and payment provider reports.

Monitor abnormal payout activity to detect fraud.

---

# Conclusion

The seller_payouts table provides a secure, transparent, and scalable financial settlement foundation for HAT-BAZAR.

It supports:

Seller earnings settlement

Commission deduction

Refund adjustments

Multiple payout methods

Financial auditing

Accounting integration

Future automated settlement workflows

This design ensures accurate seller payments while maintaining financial integrity, transparency, regulatory compliance, and long-term scalability across the marketplace ecosystem.

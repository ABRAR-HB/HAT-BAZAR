# PAYMENTS TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The payments table stores every financial transaction related to customer orders.

Each payment belongs to one order.

Each order should have one primary payment record.

Payment history must remain immutable for financial auditing.

---

# Table Name

payments

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Ownership

order_id

References:

orders.id

Each payment belongs to one order.

---

# Core Columns

id

payment_reference

order_id

customer_user_id

shop_id

payment_gateway

payment_method

transaction_id

gateway_transaction_id

currency

payment_amount

platform_fee

gateway_fee

seller_amount

payment_status

payment_type

paid_at

failed_at

refunded_at

created_at

updated_at

deleted_at

---

# Payment Status

Possible values:

- pending
- processing
- successful
- failed
- cancelled
- refunded
- partially_refunded

---

# Payment Type

Supported payment types:

- Cash on Delivery
- Wallet
- Online Payment
- Mixed Payment

Future payment methods may be added without changing the table structure.

---

# Payment Reference

Every payment receives a permanent unique reference.

Example:

HBPAY-20260708-000001

Payment references are never reused.

---
---

# Column Specifications

| Column | Data Type | Required | Notes |
|---------|-----------|----------|-------|
| id | BIGSERIAL | Yes | Primary Key |
| payment_reference | VARCHAR(50) | Yes | Unique payment reference |
| order_id | BIGINT | Yes | References orders.id |
| customer_user_id | BIGINT | Yes | References users.id |
| shop_id | BIGINT | Yes | References shops.id |
| payment_gateway | VARCHAR(50) | Yes | SSLCommerz, bKash, Nagad etc. |
| payment_method | VARCHAR(50) | Yes | COD, Wallet, Card, Mobile Banking |
| transaction_id | VARCHAR(100) | Yes | HAT-BAZAR transaction ID |
| gateway_transaction_id | VARCHAR(150) | No | Gateway transaction ID |
| currency | VARCHAR(10) | Yes | Default BDT |
| payment_amount | DECIMAL(12,2) | Yes | Total customer payment |
| platform_fee | DECIMAL(12,2) | Yes | Default 0.00 |
| gateway_fee | DECIMAL(12,2) | Yes | Default 0.00 |
| seller_amount | DECIMAL(12,2) | Yes | Net payable to seller |
| payment_status | VARCHAR(30) | Yes | Payment state |
| payment_type | VARCHAR(30) | Yes | Payment category |
| paid_at | TIMESTAMP | No | Payment completion time |
| failed_at | TIMESTAMP | No | Failure time |
| refunded_at | TIMESTAMP | No | Refund completion time |
| created_at | TIMESTAMP | Yes | UTC |
| updated_at | TIMESTAMP | Yes | UTC |
| deleted_at | TIMESTAMP | No | Soft Delete |

---

# Relationships

The payments table connects with:

- orders
- users
- shops
- wallet_transactions
- returns_refunds

Each payment belongs to exactly one order.

---

# Unique Constraints

The following fields must be unique:

- payment_reference
- transaction_id

gateway_transaction_id should also be unique whenever provided.

---

# Indexes

Create indexes for:

- order_id
- customer_user_id
- shop_id
- payment_status
- payment_method
- payment_gateway
- created_at
- paid_at

---

# Financial Calculation Rules

Formula:

seller_amount =

payment_amount

− platform_fee

− gateway_fee

seller_amount must never be negative.

All monetary values must be stored using DECIMAL to prevent floating-point errors.

---

# Payment Integrity Rules

A successful payment cannot be modified.

Refunds must create separate financial records instead of changing historical payment values.

Every payment must always be traceable from:

Customer

↓

Order

↓

Payment

↓

Settlement

↓

Wallet (if applicable)

---

# Business Rules

Each completed order should have one primary successful payment record.

Failed payments remain in the database for auditing.

Deleted payment records are not allowed.

Soft Delete should only be available for exceptional administrative purposes according to platform policy.

---
---

# Refund Integration

Refunds are handled through the dedicated Return & Refund System.

The original payment record is never modified.

Each refund creates a separate refund transaction linked to the original payment.

Supported refund types:

- Full Refund
- Partial Refund
- Wallet Refund
- Original Payment Method Refund

Every refund must maintain complete financial traceability.

---

# Seller Settlement Integration

Successful payments become eligible for seller settlement.

Seller payouts are processed separately from customer payments.

Settlement calculations use:

- Seller Amount
- Platform Commission
- Gateway Fee
- Refund Adjustments

Settlement history is stored in dedicated settlement tables.

---

# Payment Gateway Integration

The system is designed to support multiple payment gateways.

Supported gateways may include:

- SSLCommerz
- bKash
- Nagad
- Rocket
- Visa
- Mastercard

Future gateways can be added without changing the database structure.

Gateway-specific metadata should be stored separately when necessary.

---

# Security Rules

Payment records are immutable after successful completion.

Sensitive payment information must never be stored in plain text.

Card numbers, CVV, PINs, and authentication credentials must never be stored in the database.

All payment communication must use secure encrypted connections.

Every payment event should be logged for auditing purposes.

---

# Future Expansion

Future versions may support:

- Installment Payments (EMI)
- Subscription Payments
- Escrow Payments
- Split Payments
- International Currencies
- Automatic Seller Settlement
- Scheduled Payouts
- Multi-Gateway Routing
- Payment Retry Mechanism
- Fraud Detection Integration

---

# Audit & Reporting

The payments table serves as the primary source for:

- Financial Reports
- Daily Revenue Reports
- Seller Settlement Reports
- Commission Reports
- Refund Reports
- Audit Logs

Historical payment records must remain available for regulatory and business reporting.

---

# Summary

The payments table is the financial backbone of the HAT-BAZAR platform.

It ensures secure, traceable, and auditable payment processing while supporting future growth, multiple payment gateways, seller settlements, and advanced financial features without requiring major structural changes.

---

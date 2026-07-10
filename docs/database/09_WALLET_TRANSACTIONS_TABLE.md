# WALLET TRANSACTIONS TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The wallet_transactions table stores every financial transaction performed inside the HAT-BAZAR wallet system.

Every balance increase or decrease must be recorded here.

This table acts as the official financial ledger of the platform.

Nothing should change a wallet balance without creating a wallet transaction record.

---

# Table Name

wallet_transactions

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

The wallet transaction system is designed to provide:

- Full audit history
- Secure financial records
- Refund tracking
- Cashback tracking
- Seller settlement history
- Withdrawal history
- Admin adjustment history

Every wallet activity becomes permanently traceable.

Transactions are immutable.

Existing records should never be edited after completion.

If corrections are required, a new adjustment transaction must be created.

---

# Ownership

Each wallet transaction belongs to exactly one user.

Reference:

users.id

Column:

user_id

---

# Main Columns

id

wallet_reference

user_id

payment_id

order_id

refund_id

transaction_type

transaction_direction

amount

currency

balance_before

balance_after

status

description

reference_number

notes

created_by

created_at

updated_at

deleted_at

---

# Wallet Reference

Every transaction receives a globally unique reference.

Example:

HBW-20260708-000001

Reference rules

- Never duplicated
- Never reused
- Searchable
- Human readable

---

# Transaction Direction

Allowed values

credit

debit

Credit increases balance.

Debit decreases balance.

---

# Supported Transaction Types

Top Up

Order Payment

Refund

Cashback

Reward

Seller Settlement

Withdrawal

Transfer

Admin Credit

Admin Debit

Adjustment

Penalty

Bonus

Future transaction types can be added without changing database structure.

---
# Column Definitions

---

## id

Type

BIGSERIAL

Primary Key

Not Null

---

## wallet_reference

Type

VARCHAR(50)

Unique

Not Null

Indexed

Example

HBW-20260708-000001

---

## user_id

Type

BIGINT

Foreign Key

References

users(id)

Not Null

Indexed

---

## payment_id

Type

BIGINT

Nullable

References

payments(id)

Used when transaction originates from payment system.

---

## order_id

Type

BIGINT

Nullable

References

orders(id)

Used for order related wallet activities.

---

## refund_id

Type

BIGINT

Nullable

References

returns_refunds(id)

Used for refund transactions.

---

## transaction_type

Type

VARCHAR(40)

Not Null

Allowed values

Top Up

Order Payment

Refund

Cashback

Reward

Seller Settlement

Withdrawal

Transfer

Admin Credit

Admin Debit

Adjustment

Penalty

Bonus

---

## transaction_direction

Type

VARCHAR(10)

Not Null

Allowed

credit

debit

---

## amount

Type

DECIMAL(12,2)

Not Null

Minimum

0.01

Never negative

---

## currency

Type

CHAR(3)

Default

BDT

Future ready for multi-currency support.

---

## balance_before

Type

DECIMAL(12,2)

Not Null

Wallet balance before transaction execution.

---

## balance_after

Type

DECIMAL(12,2)

Not Null

Wallet balance immediately after transaction.

---

## status

Type

VARCHAR(20)

Default

completed

Allowed values

pending

processing

completed

failed

cancelled

reversed

---

## description

Type

TEXT

Nullable

Human readable transaction summary.

Example

Refund for Order HB-20458

---

## reference_number

Type

VARCHAR(100)

Nullable

Stores external references.

Examples

Payment Gateway ID

Bank Reference

Manual Adjustment Reference

---

## notes

Type

TEXT

Nullable

Internal system notes.

Hidden from customer.

---

## created_by

Type

BIGINT

Nullable

References

users(id)

Stores who initiated the transaction.

Possible creators

Customer

Seller

Admin

System

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

Soft Delete

Nullable

---

# Foreign Keys

wallet_transactions.user_id

→ users.id

wallet_transactions.payment_id

→ payments.id

wallet_transactions.order_id

→ orders.id

wallet_transactions.refund_id

→ returns_refunds.id

wallet_transactions.created_by

→ users.id

---

# Indexes

PRIMARY KEY (id)

UNIQUE (wallet_reference)

INDEX (user_id)

INDEX (order_id)

INDEX (payment_id)

INDEX (refund_id)

INDEX (transaction_type)

INDEX (status)

INDEX (created_at)

INDEX (transaction_direction)

Composite Index

(user_id, created_at)

(order_id, transaction_type)

(status, created_at)

---

# Financial Validation Rules

Amount must always be greater than zero.

Credit increases wallet balance.

Debit decreases wallet balance.

balance_after must always equal the actual wallet balance after execution.

A completed transaction must never be edited.

Failed transactions never affect wallet balance.

Cancelled transactions never affect wallet balance.

Reversed transactions must create a separate reverse entry instead of modifying the original record.

---
# Refund Integration

When an order is refunded, the system creates a new wallet transaction.

Rules

Original transaction remains unchanged.

A new Credit transaction is generated.

transaction_type

Refund

transaction_direction

credit

order_id is linked.

refund_id is linked.

Full audit history is preserved.

---

# Cashback Integration

Cashback is credited only after successful order completion.

Transaction Type

Cashback

Direction

credit

Status

completed

Cashback may have an expiration date managed separately by business rules.

---

# Seller Settlement

When seller earnings are transferred to their wallet:

transaction_type

Seller Settlement

Direction

credit

Settlement records remain permanently traceable.

---

# Withdrawal Flow

When a user withdraws wallet funds:

Step 1

Create transaction

Status

pending

Step 2

Admin or automated system verifies.

Step 3

If approved

Status

completed

Direction

debit

Step 4

If rejected

Status

cancelled

Wallet balance remains unchanged.

---

# Admin Adjustments

Admins may perform manual adjustments only with proper authorization.

Adjustment Types

Admin Credit

Admin Debit

Adjustment

Penalty

Bonus

Every adjustment must include:

Reason

Admin ID

Reference Number

Timestamp

These transactions must be logged permanently.

---

# Fraud Prevention

The system should prevent:

Negative balances

Duplicate transactions

Duplicate payment callbacks

Duplicate refunds

Multiple cashback credits

Repeated settlement processing

Wallet manipulation

Every transaction must have a unique wallet_reference.

---

# Audit Trail

Every financial operation must remain traceable.

Audit includes:

Who initiated it

Who approved it

Linked payment

Linked order

Linked refund

Timestamp

Previous balance

New balance

Transaction status

No completed transaction should ever be deleted permanently.

Soft Delete is only for exceptional administrative situations.

---

# Performance Considerations

Indexes should optimize:

Wallet history

Monthly statements

Refund lookup

Settlement reports

Financial reconciliation

Admin investigation

Customer wallet timeline

---

# Example Records

Example 1

Top Up

Amount

1000.00

Direction

credit

Before

500.00

After

1500.00

---

Example 2

Order Payment

Amount

320.00

Direction

debit

Before

1500.00

After

1180.00

---

Example 3

Refund

Amount

320.00

Direction

credit

Before

1180.00

After

1500.00

---

Example 4

Cashback

Amount

20.00

Direction

credit

Before

1500.00

After

1520.00

---

# Future Expansion

The wallet system is designed to support:

International currencies

Scheduled payouts

Merchant wallets

Escrow accounts

Referral bonuses

Gift wallet

Subscription billing

Loyalty rewards

Offline settlement

AI fraud detection

Blockchain-ready transaction export

Financial analytics

---

# Security Notes

Wallet balance must never be updated directly.

Every balance change must originate from a valid wallet transaction.

Database updates should occur inside ACID-compliant transactions.

Financial calculations must always use DECIMAL instead of FLOAT.

All wallet APIs require authentication and authorization.

---

# Conclusion

The wallet_transactions table serves as the financial ledger of the HAT-BAZAR platform.

It guarantees:

Financial transparency

Complete audit history

Accurate balance tracking

Secure refund processing

Reliable settlement management

Scalability for future financial services

This design ensures that every wallet operation is traceable, secure, and suitable for production-scale e-commerce systems.

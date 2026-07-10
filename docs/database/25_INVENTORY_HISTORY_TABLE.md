# INVENTORY HISTORY TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The inventory_history table records every inventory movement that occurs within the HAT-BAZAR platform.

It provides a complete audit trail of stock changes for accountability, financial reconciliation, operational monitoring, and inventory analysis.

Unlike the inventory table, this table never stores the current stock. Instead, it stores every inventory event permanently.

---

# Table Name

inventory_history

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Each inventory history record represents one inventory transaction.

A single inventory item may generate thousands of history records over time.

History records are immutable and should never be modified after creation.

---

# Ownership

Each history record belongs to:

One inventory

One product

One shop

Optional order

Optional return

Optional refund

Optional user

---

# Main Columns

id

history_uuid

inventory_id

product_id

shop_id

order_id

return_id

refund_id

performed_by

transaction_type

quantity

stock_before

stock_after

remarks

created_at

---

# Transaction Types

stock_in

stock_out

reservation

reservation_release

manual_adjustment

return_received

refund_adjustment

damage

loss

warehouse_transfer

initial_stock

---

# Business Rules

Every history record belongs to one inventory record.

History records are immutable.

Stock before and stock after must always be recorded.

Every inventory change generates exactly one history record.

Soft delete is not recommended.

---

# Usage

The inventory history system supports:

Inventory auditing

Stock reconciliation

Order tracking

Return tracking

Manual adjustments

Fraud investigation

Inventory analytics

---

# Audit Principle

Every inventory movement must be traceable from creation to completion.

No inventory modification should occur without generating a corresponding history record.


# Column Definitions

---

## id

Type

BIGSERIAL

Primary Key

Not Null

---

## history_uuid

Type

UUID

Not Null

Unique

System-generated globally unique inventory history identifier.

---

## inventory_id

Type

BIGINT

Foreign Key

References

inventory(id)

Not Null

Associated inventory record.

---

## product_id

Type

BIGINT

Foreign Key

References

products(id)

Not Null

Associated product.

---

## shop_id

Type

BIGINT

Foreign Key

References

shops(id)

Not Null

Owner shop of the inventory.

---

## order_id

Type

BIGINT

Foreign Key

References

orders(id)

Nullable

Associated order, if applicable.

---

## return_id

Type

BIGINT

Foreign Key

References

returns(id)

Nullable

Associated return request.

---

## refund_id

Type

BIGINT

Foreign Key

References

refunds(id)

Nullable

Associated refund event, if applicable.

---

## performed_by

Type

BIGINT

Foreign Key

References

users(id)

Nullable

User or administrator who performed the inventory action.

---

## transaction_type

Type

VARCHAR(30)

Not Null

Allowed Values

stock_in

stock_out

reservation

reservation_release

manual_adjustment

return_received

refund_adjustment

damage

loss

warehouse_transfer

initial_stock

---

## quantity

Type

INTEGER

Not Null

Quantity changed during the transaction.

---

## stock_before

Type

INTEGER

Not Null

Inventory quantity before the transaction.

---

## stock_after

Type

INTEGER

Not Null

Inventory quantity after the transaction.

---

## remarks

Type

TEXT

Nullable

Optional explanation for the inventory movement.

---

## created_at

TIMESTAMP

Default CURRENT_TIMESTAMP

---

# Foreign Keys

inventory_history.inventory_id

→ inventory.id

inventory_history.product_id

→ products.id

inventory_history.shop_id

→ shops.id

inventory_history.order_id

→ orders.id

inventory_history.return_id

→ returns.id

inventory_history.refund_id

→ refunds.id

inventory_history.performed_by

→ users.id

---

# Constraints

History UUID must be unique.

Quantity must be greater than zero.

Stock before and stock after cannot be negative.

Every inventory modification must create one history record.

History records cannot be edited after creation.

History records cannot be deleted.

---

# Indexes

PRIMARY KEY (id)

UNIQUE INDEX (history_uuid)

INDEX (inventory_id)

INDEX (product_id)

INDEX (shop_id)

INDEX (transaction_type)

INDEX (created_at)

Composite Index

(product_id, created_at)

(shop_id, created_at)

(inventory_id, created_at)

---

# Inventory Change Rules

Every inventory change must record:

Transaction type

Quantity changed

Stock before

Stock after

User responsible

Reference to related business entities where applicable

The history table serves as the permanent audit log for all inventory operations.

---

# Audit Requirements

Every stock movement must be traceable.

Historical records must remain immutable.

Audit logs must support financial reconciliation.

Inventory history should support dispute resolution and fraud investigations.

---

# Security Rules

Only authorized system processes and privileged users may create inventory history records.

Existing history records must never be modified.

Access to inventory history should be restricted according to user roles.

Sensitive operational data should be included in backup and disaster recovery procedures.

# Inventory Workflow

Every inventory operation follows a controlled workflow.

Stock Added

↓

Inventory Updated

↓

History Record Created

↓

Audit Log Updated

↓

Reports Updated

For customer orders:

Order Placed

↓

Stock Reserved

↓

History Record Created

↓

Order Delivered

↓

Stock Deducted

↓

History Record Created

For returns:

Return Approved

↓

Product Received

↓

Stock Increased

↓

History Record Created

---

# Example Records

## Example 1

History UUID

550e8400-e29b-41d4-a716-446655440777

Inventory ID

25

Product ID

105

Transaction Type

stock_out

Quantity

5

Stock Before

150

Stock After

145

Related Order

1025

Created At

2026-07-09 14:20:00

---

## Example 2

History UUID

91c74120-1ab7-4f2d-85aa-2f4c7b8d8faa

Inventory ID

25

Product ID

105

Transaction Type

return_received

Quantity

2

Stock Before

145

Stock After

147

Related Return

58

Created At

2026-07-10 11:05:00

---

# Analytics & Reporting

Inventory history supports:

Inventory movement reports

Daily stock changes

Monthly stock summaries

Fast-moving products

Slow-moving products

Stock loss analysis

Manual adjustment reports

Inventory valuation history

Fraud detection

Operational auditing

---

# Future Expansion

Future versions may support:

Multi-warehouse movement history

Batch and lot tracking

Expiry tracking

Barcode and QR code integration

RFID inventory monitoring

AI-powered inventory forecasting

Automatic anomaly detection

Supplier purchase history integration

Cross-warehouse synchronization

---

# Best Practices

Create one immutable history record for every inventory change.

Never modify existing history records.

Store complete before-and-after stock values.

Record the user or system responsible for each change.

Use database transactions to ensure inventory and history remain synchronized.

Optimize indexes for reporting and auditing.

Regularly back up historical inventory data.

---

# Conclusion

The inventory_history table provides a complete, immutable, and auditable history of every inventory movement within HAT-BAZAR.

It supports:

Inventory auditing

Order tracking

Return and refund integration

Operational reporting

Fraud investigation

Financial reconciliation

Future warehouse expansion

This design ensures full traceability of inventory changes while maintaining data integrity, accountability, and long-term scalability across the marketplace platform.


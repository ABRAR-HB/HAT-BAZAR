# INVENTORY TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The inventory table manages the available stock for every product listed on the HAT-BAZAR platform.

It maintains real-time inventory levels, reserved stock, stock availability, and operational controls to prevent overselling while supporting future warehouse expansion.

---

# Table Name

inventory

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Each inventory record represents the current stock status of one product.

Inventory is automatically updated after purchases, cancellations, returns, refunds, stock adjustments, and manual restocking.

Future versions may support multiple warehouse locations.

---

# Ownership

Each inventory record belongs to:

One product

One shop

Optional warehouse

---

# Main Columns

id

inventory_uuid

product_id

shop_id

warehouse_id

current_stock

reserved_stock

available_stock

minimum_stock_level

maximum_stock_level

reorder_level

inventory_status

last_restocked_at

created_at

updated_at

deleted_at

---

# Inventory Status

in_stock

low_stock

out_of_stock

reserved

discontinued

inactive

---

# Business Rules

Every inventory record belongs to one product.

Current stock cannot be negative.

Reserved stock cannot exceed current stock.

Available stock is calculated automatically.

Soft delete must be supported.

Inventory updates must be transactional.

---

# Usage

The inventory system supports:

Real-time stock management

Order reservation

Stock deduction

Restocking

Low stock alerts

Inventory reporting

Future warehouse management

---

# Stock Formula

Available Stock

=

Current Stock

−

Reserved Stock


# Column Definitions

---

## id

Type

BIGSERIAL

Primary Key

Not Null

---

## inventory_uuid

Type

UUID

Not Null

Unique

System-generated globally unique inventory identifier.

---

## product_id

Type

BIGINT

Foreign Key

References

products(id)

Not Null

Indexed

Associated product.

---

## shop_id

Type

BIGINT

Foreign Key

References

shops(id)

Not Null

Indexed

Owner shop of the inventory.

---

## warehouse_id

Type

BIGINT

Foreign Key

References

warehouses(id)

Nullable

Reserved for future multi-warehouse support.

---

## current_stock

Type

INTEGER

Default

0

Current physical stock quantity.

---

## reserved_stock

Type

INTEGER

Default

0

Stock reserved for pending orders.

---

## available_stock

Type

INTEGER

Generated / Computed

Available for purchase.

Formula:

Current Stock − Reserved Stock

---

## minimum_stock_level

Type

INTEGER

Default

5

Minimum acceptable inventory level.

---

## maximum_stock_level

Type

INTEGER

Nullable

Maximum storage capacity.

---

## reorder_level

Type

INTEGER

Default

10

Stock level that triggers a restocking alert.

---

## inventory_status

Type

VARCHAR(20)

Default

in_stock

Allowed Values

in_stock

low_stock

out_of_stock

reserved

discontinued

inactive

---

## last_restocked_at

Type

TIMESTAMP

Nullable

Most recent inventory replenishment time.

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

inventory.product_id

→ products.id

inventory.shop_id

→ shops.id

inventory.warehouse_id

→ warehouses.id

---

# Constraints

Inventory UUID must be unique.

Current stock cannot be negative.

Reserved stock cannot exceed current stock.

Available stock cannot be negative.

Reorder level cannot be negative.

Soft deleted inventory remains available for audit purposes.

---

# Indexes

PRIMARY KEY (id)

UNIQUE INDEX (inventory_uuid)

INDEX (product_id)

INDEX (shop_id)

INDEX (inventory_status)

INDEX (last_restocked_at)

Composite Index

(shop_id, inventory_status)

(product_id, inventory_status)

---

# Stock Reservation Logic

When an order is placed:

Reserved Stock increases.

Available Stock decreases.

When payment fails or the order is cancelled:

Reserved Stock decreases.

Available Stock increases.

When the order is completed:

Current Stock decreases.

Reserved Stock decreases.

---

# Restocking Rules

Inventory may be increased through:

Manual stock entry

Supplier purchase

Product return

Stock adjustment

Warehouse transfer (Future)

Every stock increase should generate an inventory history record.

---

# Security & Audit Requirements

Only authorized shop owners and administrators may modify inventory.

All inventory changes must be logged.

Inventory operations should be executed inside database transactions.

Historical inventory records should never be permanently deleted.


# Inventory Adjustment Workflow

Inventory levels change through controlled business events.

Examples include:

Manual stock addition

Manual stock reduction

Customer order placement

Order cancellation

Successful delivery

Customer return

Stock correction

Warehouse transfer (Future)

Every inventory adjustment should create a corresponding inventory history record for auditing.

---

# Low Stock Alert System

The platform continuously monitors inventory levels.

When Available Stock falls below the Reorder Level:

A low stock alert is generated.

The shop owner receives a notification.

Administrators may also receive alerts for critical inventory shortages.

If Available Stock reaches zero:

The product is automatically marked as Out of Stock.

Customers should no longer be allowed to place new orders for that product.

---

# Analytics & Reporting

Inventory data supports business intelligence.

Examples include:

Current inventory by shop

Low stock products

Out-of-stock products

Fast-moving products

Slow-moving products

Inventory turnover

Restocking frequency

Inventory valuation

These reports help improve purchasing and stock planning.

---

# Example Records

## Example 1

Inventory UUID

550e8400-e29b-41d4-a716-446655440666

Product ID

105

Shop ID

12

Current Stock

150

Reserved Stock

20

Available Stock

130

Inventory Status

in_stock

---

## Example 2

Inventory UUID

91c74120-1ab7-4f2d-85aa-2f4c7b8d8fee

Product ID

208

Shop ID

15

Current Stock

5

Reserved Stock

2

Available Stock

3

Inventory Status

low_stock

---

# Future Expansion

Future versions may support:

Multiple warehouses

Batch and lot tracking

Expiry date management

Barcode and QR code integration

Automatic supplier purchase orders

AI demand forecasting

Inventory synchronization across branches

Warehouse transfers

RFID inventory tracking

IoT-enabled smart inventory

---

# Best Practices

Always perform inventory updates inside database transactions.

Prevent overselling by reserving stock immediately after order placement.

Maintain a complete inventory history.

Use automatic alerts for low stock products.

Never permanently delete production inventory records.

Regularly reconcile physical inventory with system records.

Optimize inventory indexes for fast product lookup.

---

# Conclusion

The inventory table provides a secure, scalable, and production-ready inventory management foundation for HAT-BAZAR.

It supports:

Real-time stock management

Stock reservation

Automatic availability calculation

Low stock monitoring

Restocking operations

Inventory analytics

Future multi-warehouse expansion

This design ensures accurate inventory control, prevents overselling, and supports long-term operational growth across the marketplace ecosystem.

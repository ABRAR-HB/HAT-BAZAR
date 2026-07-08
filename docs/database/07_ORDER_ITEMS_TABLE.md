# ORDER_ITEMS TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The order_items table stores every product included in an order.

Each record represents one purchased product.

An order may contain multiple order items.

This table preserves product information at the time of purchase, ensuring order history remains accurate even if product details change later.

---

# Table Name

order_items

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Ownership

Each order item belongs to:

- One order
- One product
- One seller

---

# Core Columns

id

order_id

product_id

shop_id

seller_user_id

product_name

product_slug

product_sku

product_image

unit_price

quantity

discount_amount

final_unit_price

line_total

currency

created_at

updated_at

deleted_at

---

# Product Snapshot

The following values are copied from the product at purchase time:

- Product Name
- Product Slug
- SKU
- Product Image
- Unit Price

These snapshot values never change after the order is placed.

---

# Quantity Rules

Quantity must always be greater than zero.

Fractional quantities are not allowed unless supported by the product category in the future.

---

# Pricing Rules

Each order item stores:

- Original Unit Price
- Discount Amount
- Final Unit Price
- Line Total

This ensures historical pricing remains accurate.

---
---

# Column Specifications

| Column | Data Type | Required | Notes |
|---------|-----------|----------|-------|
| id | BIGSERIAL | Yes | Primary Key |
| order_id | BIGINT | Yes | References orders.id |
| product_id | BIGINT | Yes | References products.id |
| shop_id | BIGINT | Yes | References shops.id |
| seller_user_id | BIGINT | Yes | References users.id |
| product_name | VARCHAR(255) | Yes | Product snapshot |
| product_slug | VARCHAR(255) | Yes | Product snapshot |
| product_sku | VARCHAR(100) | Yes | Product snapshot |
| product_image | TEXT | Yes | Primary image snapshot |
| unit_price | DECIMAL(12,2) | Yes | Original unit price |
| quantity | INTEGER | Yes | Purchased quantity |
| discount_amount | DECIMAL(12,2) | Yes | Default 0.00 |
| final_unit_price | DECIMAL(12,2) | Yes | Price after discount |
| line_total | DECIMAL(12,2) | Yes | Final total for this item |
| currency | VARCHAR(10) | Yes | Default BDT |
| created_at | TIMESTAMP | Yes | UTC |
| updated_at | TIMESTAMP | Yes | UTC |
| deleted_at | TIMESTAMP | No | Soft Delete |

---

# Relationships

The order_items table connects with:

- orders
- products
- shops
- users (seller)

Each order can contain many order items.

Each order item belongs to exactly one order.

---

# Unique Constraints

There are no globally unique business fields.

Multiple rows may reference the same product across different orders.

---

# Indexes

Create indexes for:

- order_id
- product_id
- shop_id
- seller_user_id
- created_at

---

# Calculation Rules

Formula:

line_total =

final_unit_price × quantity

Where:

final_unit_price =

unit_price − discount_amount

The calculated values must never be negative.

---

# Product Integrity Rules

After an order is placed:

- Product Name snapshot never changes.
- Product Image snapshot never changes.
- Product SKU snapshot never changes.
- Product Price snapshot never changes.

Editing the original product must not affect previous order records.

---

# Business Rules

Every order item must reference a valid:

- Order
- Product
- Shop
- Seller

Deleting a product must never remove historical order items.

Historical purchase records are permanent.

---
---

# Refund Integration

The order_items table supports both full and partial refunds.

Refund processing is handled by the Return & Refund System.

Each refund request references the affected order item(s).

This design allows refunds for individual products without affecting the entire order.

Order item records are never modified after purchase.

Refund information is stored in dedicated refund tables.

Only refund-related status and references are linked to the original order item.

Historical pricing, quantity, and product snapshot data remain unchanged.

Every refund transaction must be recorded for financial auditing.

Future versions may support:

- Partial Quantity Refund
- Partial Amount Refund
- Exchange Requests
- Replacement Requests
- Refund Approval Workflow
- Refund Rejection Workflow

This design ensures complete financial traceability while preserving historical order integrity.

---

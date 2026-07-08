# ORDERS TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The orders table stores every customer purchase made on the HAT-BAZAR platform.

Each order belongs to one customer.

An order may contain one or multiple products.

Order items are stored separately in the order_items table.

---

# Table Name

orders

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Ownership

customer_user_id

References:

users.id

Each order belongs to one customer.

---

# Core Columns

id

order_number

customer_user_id

shop_id

payment_id

wallet_transaction_id

subtotal

discount_amount

coupon_discount

delivery_fee

platform_fee

tax_amount

grand_total

currency

payment_method

payment_status

order_status

delivery_status

delivery_address

delivery_phone

delivery_notes

estimated_delivery_date

completed_at

cancelled_at

created_at

updated_at

deleted_at

---

# Payment Status

Possible values:

- pending
- paid
- partially_paid
- failed
- refunded

---

# Order Status

Possible values:

- pending
- confirmed
- processing
- packed
- shipped
- delivered
- completed
- cancelled
- returned
- refunded

---

# Delivery Status

Possible values:

- waiting
- assigned
- picked_up
- in_transit
- delivered
- failed

---

# Order Number

Every order receives a unique order number.

Example:

HB-20260707-000001

Order numbers are permanent and never reused.

---
---

# Column Specifications

| Column | Data Type | Required | Notes |
|---------|-----------|----------|-------|
| id | BIGSERIAL | Yes | Primary Key |
| order_number | VARCHAR(50) | Yes | Unique Order Number |
| customer_user_id | BIGINT | Yes | References users.id |
| shop_id | BIGINT | Yes | References shops.id |
| payment_id | BIGINT | No | References payments.id |
| wallet_transaction_id | BIGINT | No | References wallet_transactions.id |
| subtotal | DECIMAL(12,2) | Yes | Product total before discounts |
| discount_amount | DECIMAL(12,2) | Yes | Default 0.00 |
| coupon_discount | DECIMAL(12,2) | Yes | Default 0.00 |
| delivery_fee | DECIMAL(12,2) | Yes | Default 0.00 |
| platform_fee | DECIMAL(12,2) | Yes | Default 0.00 |
| tax_amount | DECIMAL(12,2) | Yes | Default 0.00 |
| grand_total | DECIMAL(12,2) | Yes | Final payable amount |
| currency | VARCHAR(10) | Yes | Default BDT |
| payment_method | VARCHAR(30) | Yes | COD, Wallet, SSLCommerz etc. |
| payment_status | VARCHAR(30) | Yes | Payment state |
| order_status | VARCHAR(30) | Yes | Order lifecycle |
| delivery_status | VARCHAR(30) | Yes | Delivery progress |
| delivery_address | TEXT | Yes | Shipping address |
| delivery_phone | VARCHAR(20) | Yes | Customer phone |
| delivery_notes | TEXT | No | Optional instructions |
| estimated_delivery_date | DATE | No | Estimated arrival |
| completed_at | TIMESTAMP | No | Completion time |
| cancelled_at | TIMESTAMP | No | Cancellation time |
| created_at | TIMESTAMP | Yes | UTC |
| updated_at | TIMESTAMP | Yes | UTC |
| deleted_at | TIMESTAMP | No | Soft Delete |

---

# Unique Constraints

The following field must be unique:

- order_number

---

# Indexes

Create indexes for:

- customer_user_id
- shop_id
- payment_id
- order_status
- payment_status
- delivery_status
- created_at
- completed_at

---

# Relationships

The orders table connects with:

- users
- shops
- payments
- wallet_transactions
- order_items
- riders
- notifications
- returns_refunds

---

# Order Calculation Rules

Formula:

grand_total =

subtotal

− discount_amount

− coupon_discount

+ delivery_fee

+ platform_fee

+ tax_amount

The final amount must never be negative.

---

# Payment Rules

An order cannot move to Completed unless payment requirements are satisfied.

Supported payment methods:

- Cash on Delivery (COD)
- Wallet
- SSLCommerz
- bKash
- Nagad
- Rocket
- Bank Transfer

Additional payment gateways may be added in the future without changing the table structure.

---
---

# Order Workflow

Every order follows a controlled lifecycle.

Customer places order

↓

Pending

↓

Confirmed

↓

Processing

↓

Packed

↓

Shipped

↓

Delivered

↓

Completed

If any problem occurs:

↓

Cancelled

or

Returned

↓

Refunded

Every status transition must be recorded in the audit log.

---

# Cancellation Rules

Customers may cancel an order only before it enters the Processing stage.

Sellers may request cancellation under platform policy.

Administrators may cancel orders when required for security, fraud prevention, or policy enforcement.

Cancelled orders cannot be restored.

---

# Return & Refund Integration

Returned and refunded orders are managed by the Return & Refund System.

The orders table stores only the final order status.

Detailed return requests are stored in dedicated tables.

A refunded order must always reference its payment record.

---

# Rider Integration

Each delivery order may be assigned to one rider.

Rider assignment is stored in a dedicated rider assignment table.

Delivery status updates automatically synchronize with the orders table.

---

# Notification Integration

Customers receive notifications for:

- Order Confirmed
- Order Processing
- Order Shipped
- Rider Assigned
- Out for Delivery
- Delivered
- Cancelled
- Refunded

Sellers receive notifications for:

- New Order
- Cancellation
- Return Request
- Refund Request

---

# Business Rules

Every order must belong to:

- One customer
- One shop

Order records are permanent financial records.

Orders must never be permanently deleted after successful payment.

Soft Delete should only be used for administrative purposes where permitted by platform policy.

---

# Future Expansion

The orders table supports future features including:

- Split Orders
- Multi-vendor Checkout
- Scheduled Delivery
- Express Delivery
- Partial Shipment
- Partial Refund
- International Shipping
- AI Delivery Prediction

These features can be added without changing the core table structure.

---

# Security Notes

Only authorized users may view order details.

Customers can access only their own orders.

Sellers can access only orders belonging to their own shops.

Administrative actions affecting orders must always be recorded in the audit log.

---

# Summary

The orders table is the core transaction table of the HAT-BAZAR platform.

It connects customers, shops, products, payments, wallets, riders, notifications, reviews, and return/refund systems while maintaining complete financial and operational integrity.

---

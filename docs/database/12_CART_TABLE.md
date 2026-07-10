# CART TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The cart table stores products that customers add before placing an order.

It allows users to collect products from one or more shops, review pricing, apply discounts, and proceed to checkout.

The cart acts as a temporary workspace before an order is created.

---

# Table Name

cart

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

The cart table represents individual cart items.

Each row corresponds to one product added by a customer.

If the same product is added again, the quantity should be updated instead of creating duplicate rows.

---

# Ownership

Each cart item belongs to:

- One customer
- One shop
- One product

Guest carts may be supported in the future through temporary session identifiers.

---

# Main Columns

id

user_id

shop_id

product_id

quantity

unit_price

discount_amount

subtotal

currency

status

expires_at

created_at

updated_at

deleted_at

---

# Business Rules

Only active products can be added to the cart.

Quantity cannot exceed available stock.

Cart prices are snapshots and must be revalidated during checkout.

Each customer may have only one active cart entry for the same product.

Inactive or expired cart items are ignored during checkout.

---

# Cart Lifecycle

Product Added

↓

Quantity Updated

↓

Checkout

↓

Order Created

↓

Cart Cleared

---

# Cart Expiration

Cart items should expire automatically after a configurable period of inactivity.

Recommended default:

30 days

Expired items may be removed by scheduled cleanup jobs.

---
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

Indexed

Owner of the cart item.

---

## shop_id

Type

BIGINT

Foreign Key

References

shops(id)

Not Null

Indexed

Shop that sells the product.

---

## product_id

Type

BIGINT

Foreign Key

References

products(id)

Not Null

Indexed

Product added to the cart.

---

## quantity

Type

INTEGER

Not Null

Default

1

Database Constraint

CHECK (quantity > 0)

---

## unit_price

Type

NUMERIC(12,2)

Not Null

Price of the product when it was added to the cart.

Used as a temporary snapshot.

---

## discount_amount

Type

NUMERIC(12,2)

Default

0.00

Stores product-level discount at the time of adding to cart.

---

## subtotal

Type

NUMERIC(12,2)

Not Null

Formula

(quantity × unit_price) − discount_amount

Revalidated during checkout.

---

## currency

Type

VARCHAR(10)

Default

BDT

Supports future multi-currency expansion.

---

## status

Type

VARCHAR(20)

Default

active

Allowed Values

active

checked_out

expired

removed

saved_for_later

---

## expires_at

Type

TIMESTAMP

Nullable

Automatically assigned based on cart expiration policy.

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

cart.user_id

→ users.id

cart.shop_id

→ shops.id

cart.product_id

→ products.id

---

# Constraints

Only one active cart entry per user per product.

Quantity must always be greater than zero.

Product must exist and be active.

Shop must exist.

Subtotal cannot be negative.

Expired cart items cannot proceed to checkout.

---

# Indexes

PRIMARY KEY (id)

INDEX (user_id)

INDEX (shop_id)

INDEX (product_id)

INDEX (status)

INDEX (expires_at)

Composite Index

(user_id, product_id)

(user_id, status)

(shop_id, status)

---

# Price Snapshot Rules

The cart stores the product price at the time it is added.

At checkout:

Current product price is fetched again.

If the price has changed:

Customer is notified.

Cart subtotal is recalculated.

Customer must confirm the updated price before completing payment.

---

# Stock Validation

Stock is checked:

When adding a product

When updating quantity

Again during checkout

If available stock is insufficient:

Requested quantity is rejected or adjusted according to business rules.

---
# Multi-Shop Cart Rules

HAT-BAZAR supports adding products from multiple shops into one cart.

Rules

Products remain grouped by shop.

Shipping charges are calculated separately for each shop.

Each shop creates its own order during checkout.

Customers complete payment through a single checkout process.

---

# Guest Cart

Future versions may support guest shopping.

Guest carts are linked using a temporary session identifier.

After login:

Guest cart items are merged into the customer's cart.

If duplicate products exist:

Quantities are merged while respecting stock limits.

---

# Checkout Integration

Before creating an order, the system must verify:

Product availability

Latest product price

Shop availability

Product status

Purchase limits

If validation succeeds:

Cart items are converted into order_items.

Cart status changes to:

checked_out

---

# Cart Recovery

If checkout fails:

Cart remains active.

Customers may retry checkout after correcting any issues.

Expired cart items may be restored within the allowed recovery period.

---

# Saved For Later

Customers may move items from the active cart to a "Saved for Later" list.

Rules

Saved items are excluded from checkout.

Prices and stock are revalidated when moved back to the active cart.

---

# Example Records

Example 1

User ID

25

Product ID

410

Quantity

2

Unit Price

550.00

Subtotal

1100.00

Status

active

---

Example 2

User ID

25

Product ID

512

Quantity

1

Unit Price

899.00

Discount

100.00

Subtotal

799.00

Status

saved_for_later

---

# Security Rules

Customers may only access their own cart.

Cart items cannot be transferred between users.

Prices stored in the cart are informational only.

Final payable amount must always be recalculated during checkout.

Cart APIs must require authentication.

All cart updates should be validated on the server.

Client-side values must never be trusted.

---

# Performance Considerations

Frequently accessed cart data should be optimized with indexes.

Cart totals should be calculated efficiently.

Expired carts should be cleaned periodically by background jobs.

Product stock should not be permanently reserved while items remain in the cart.

---

# Future Expansion

The cart system is designed to support:

Coupon integration

Gift wrapping

Gift messages

Buy Now feature

Wishlist to Cart

AI product recommendations

Cart sharing

Abandoned cart reminders

Cross-sell recommendations

Recently viewed products

---

# Best Practices

Never create duplicate active cart entries for the same user and product.

Always validate stock before checkout.

Always validate prices before payment.

Use soft delete for auditability.

Keep business logic inside the application layer.

---

# Conclusion

The cart table provides a scalable and production-ready shopping cart system for HAT-BAZAR.

It supports:

Multi-shop purchasing

Reliable checkout preparation

Price validation

Stock validation

Guest cart expansion

Future marketplace features

This design ensures a fast, secure, and user-friendly shopping experience while remaining scalable for future growth.

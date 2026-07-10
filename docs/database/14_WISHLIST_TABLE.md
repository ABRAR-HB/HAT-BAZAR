# WISHLIST TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The wishlist table allows customers to save products for future purchase without adding them to the shopping cart.

Wishlist helps customers remember products they are interested in and improves engagement by enabling personalized recommendations, price alerts, and promotional notifications.

---

# Table Name

wishlists

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Each wishlist record represents one product saved by one user.

The same product cannot exist more than once in the same user's wishlist.

Removing a product from the wishlist does not affect the shopping cart or previous orders.

---

# Ownership

Each wishlist item belongs to:

- One user
- One product

One user can save many products.

One product can appear in many users' wishlists.

---

# Main Columns

id

user_id

product_id

shop_id

added_at

priority

notes

status

created_at

updated_at

deleted_at

---

# Business Rules

Only authenticated users can use the wishlist.

Duplicate wishlist entries are not allowed.

Soft delete must be supported.

Wishlist items remain available until removed by the customer or the product becomes permanently unavailable.

---

# Wishlist Usage

Wishlist is used for:

Future purchases

Price drop alerts

Back-in-stock notifications

Personalized recommendations

Marketing campaigns

Recently viewed suggestions

---

# Supported Status

active

removed

archived

---

# Customer Experience

Customers can:

Save products

Remove products

Move products to cart

View saved products

Receive notifications when prices change

Receive notifications when products are restocked

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

Owner of the wishlist item.

---

## product_id

Type

BIGINT

Foreign Key

References

products(id)

Not Null

Indexed

Saved product.

---

## shop_id

Type

BIGINT

Foreign Key

References

shops(id)

Not Null

Indexed

Shop that owns the product.

---

## added_at

Type

TIMESTAMP

Default

CURRENT_TIMESTAMP

Time when the product was added to the wishlist.

---

## priority

Type

SMALLINT

Default

1

Nullable

Optional customer priority.

Example

1 = Normal

2 = High

3 = Very High

---

## notes

Type

VARCHAR(255)

Nullable

Optional personal note.

Example

Buy next month

Gift for brother

Wait for discount

---

## status

Type

VARCHAR(20)

Default

active

Allowed Values

active

removed

archived

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

wishlists.user_id

→ users.id

wishlists.product_id

→ products.id

wishlists.shop_id

→ shops.id

---

# Constraints

Each wishlist item must belong to one user.

Each wishlist item must reference one product.

The same user cannot save the same product twice.

Only active products should appear in customer wishlist pages.

Deleted products remain for historical analytics but are hidden from customers.

---

# Unique Constraint

(user_id, product_id)

This prevents duplicate wishlist entries.

---

# Indexes

PRIMARY KEY (id)

INDEX (user_id)

INDEX (product_id)

INDEX (shop_id)

INDEX (status)

Composite Index

(user_id, status)

(shop_id, product_id)

---

# Wishlist to Cart Flow

Customers may move wishlist items directly into the shopping cart.

Before moving:

Product must exist.

Product must be active.

Stock must be available.

Purchase limits must be verified.

If successful:

Item is added to the cart.

Wishlist item may remain or be removed depending on application settings.

---

# Product Availability Rules

Wishlist items are not reservations.

Saving a product does not reduce inventory.

If a product becomes unavailable:

Display "Out of Stock."

If permanently deleted:

Hide the product while keeping the wishlist record for analytics.

---

# Security & Privacy

Customers can access only their own wishlist.

Wishlist APIs require authentication.

Server-side validation is required before every operation.

Client-side product information must never be trusted.

Soft deleted wishlist items remain available for auditing.

# Price Drop Notification Integration

The wishlist system integrates with the notification service.

When a saved product price decreases:

The system detects the new price.

Customers who saved the product receive a notification.

Notification examples:

Price Dropped

Limited Time Offer

Flash Sale

Coupon Available

---

# Back-in-Stock Integration

If an out-of-stock product becomes available again:

Inventory service updates stock.

Wishlist service identifies interested customers.

Notifications are automatically sent.

Example

"The product you saved is back in stock."

---

# Recommendation Engine Integration

Wishlist data improves recommendation quality.

Possible recommendation sources:

Products in the same category

Similar brands

Frequently purchased together

Trending products

Recently viewed products

AI-powered personalized suggestions

Wishlist popularity score

---

# Analytics

Wishlist data supports business intelligence.

Useful metrics include:

Most wishlisted products

Most wishlisted categories

Conversion rate from wishlist to purchase

Average wishlist size

Wishlist abandonment rate

Top performing shops

Seasonal trends

---

# Example Records

## Example 1

User ID

25

Product ID

108

Shop ID

7

Priority

3

Notes

Buy during Eid sale

Status

active

---

## Example 2

User ID

41

Product ID

220

Shop ID

15

Priority

1

Notes

Gift for sister

Status

active

---

# Future Expansion

Future versions may support:

Wishlist folders

Public wishlists

Shared wishlists

Family wishlists

Gift registries

One-click purchase

Price history charts

Expected price prediction

AI shopping assistant

---

# Best Practices

Use soft delete instead of permanent deletion.

Prevent duplicate wishlist entries.

Never reserve stock for wishlist items.

Cache frequently accessed wishlist data.

Send notifications responsibly to avoid spam.

Validate product availability before moving to cart.

Maintain historical analytics even after product removal.

---

# Conclusion

The wishlist table provides a scalable and customer-friendly product saving system for HAT-BAZAR.

It supports:

Product saving

Wishlist-to-cart conversion

Price drop alerts

Back-in-stock notifications

Recommendation engine integration

Business analytics

Future AI-powered shopping experiences

This design improves customer engagement while providing valuable insights for sellers and administrators.

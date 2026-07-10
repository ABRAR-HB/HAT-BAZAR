# COUPONS TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The coupons table manages all discount coupons available within the HAT-BAZAR platform.

Coupons help increase customer engagement, boost sales, promote specific shops or products, and reward loyal customers through promotional campaigns.

The system supports both platform-wide coupons and shop-specific coupons.

---

# Table Name

coupons

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Each coupon represents one promotional offer.

Coupons may be created by:

Platform administrators

Individual shop owners

Automated marketing campaigns (Future)

Coupons can provide either a fixed discount or a percentage discount.

---

# Ownership

Each coupon belongs to:

Platform

OR

One shop

A platform coupon is available across the marketplace.

A shop coupon is valid only for products from that shop.

---

# Main Columns

id

coupon_code

shop_id

created_by

coupon_type

discount_type

discount_value

minimum_order_amount

maximum_discount_amount

usage_limit

usage_per_user

starts_at

expires_at

status

created_at

updated_at

deleted_at

---

# Coupon Types

platform

shop

campaign

referral

loyalty

welcome

festival

---

# Discount Types

fixed

percentage

free_shipping (Future)

cashback (Future)

---

# Business Rules

Coupon codes must be unique.

Coupons support soft delete.

Expired coupons cannot be used.

Coupons may have usage limits.

Coupons may have per-user usage limits.

Coupons cannot be applied to cancelled orders.

---

# Usage

The coupon system supports:

Platform promotions

Shop promotions

Festival campaigns

Referral rewards

Welcome offers

Loyalty rewards

Future cashback campaigns

---

# Supported Status

active

inactive

expired

deleted

scheduled
# Column Definitions

---

## id

Type

BIGSERIAL

Primary Key

Not Null

---

## coupon_code

Type

VARCHAR(50)

Not Null

Unique

Customer-entered coupon code.

Example

EID2026

WELCOME100

SHOP10

---

## shop_id

Type

BIGINT

Foreign Key

References

shops(id)

Nullable

NULL indicates a platform-wide coupon.

---

## created_by

Type

BIGINT

Foreign Key

References

users(id)

Not Null

Administrator or shop owner who created the coupon.

---

## coupon_type

Type

VARCHAR(30)

Not Null

Allowed Values

platform

shop

campaign

referral

loyalty

welcome

festival

---

## discount_type

Type

VARCHAR(20)

Not Null

Allowed Values

fixed

percentage

free_shipping

cashback

---

## discount_value

Type

DECIMAL(12,2)

Not Null

Discount amount or percentage.

Examples

100.00

10.00

25.00

---

## minimum_order_amount

Type

DECIMAL(12,2)

Default

0

Minimum order value required.

---

## maximum_discount_amount

Type

DECIMAL(12,2)

Nullable

Used only for percentage discounts.

---

## usage_limit

Type

INTEGER

Nullable

Maximum total coupon usage.

---

## usage_per_user

Type

INTEGER

Default

1

Maximum usage allowed per customer.

---

## starts_at

Type

TIMESTAMP

Not Null

Coupon activation time.

---

## expires_at

Type

TIMESTAMP

Not Null

Coupon expiration time.

---

## status

Type

VARCHAR(20)

Default

scheduled

Allowed Values

scheduled

active

inactive

expired

deleted

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

coupons.shop_id

→ shops.id

coupons.created_by

→ users.id

---

# Constraints

Coupon codes must be unique.

Start time must be earlier than expiration time.

Discount value must be greater than zero.

Percentage discounts cannot exceed 100%.

Maximum discount applies only to percentage coupons.

Usage limits cannot be negative.

Deleted coupons cannot be redeemed.

---

# Indexes

PRIMARY KEY (id)

UNIQUE INDEX (coupon_code)

INDEX (shop_id)

INDEX (coupon_type)

INDEX (status)

INDEX (expires_at)

Composite Index

(shop_id, status)

(coupon_type, status)

(status, expires_at)

---

# Coupon Validation Logic

Before applying a coupon, the system verifies:

Coupon exists.

Coupon is active.

Current time is within the valid period.

Usage limit has not been exceeded.

Per-user usage limit has not been exceeded.

Minimum order amount is satisfied.

Coupon belongs to the correct shop (if applicable).

Coupon is compatible with the current order.

---

# Usage Limits

Platform administrators may define:

Maximum global usage

Maximum usage per customer

Campaign duration

Applicable products or categories (Future)

Applicable customer groups (Future)

---

# Security & Fraud Prevention

Coupon validation must always occur on the server.

Coupon codes should not be predictable.

Expired or deleted coupons must always be rejected.

Repeated invalid redemption attempts may be rate-limited.

Coupon usage should be logged for fraud detection and auditing.
# Order Integration

The coupon system integrates directly with the order processing service.

Before an order is confirmed, the system validates the selected coupon.

If valid:

The discount is calculated.

The order total is updated.

The coupon usage counter is increased.

The customer's usage history is recorded.

If the order is cancelled before payment, the coupon usage may be restored according to business rules.

---

# Campaign Integration

Coupons may be created as part of marketing campaigns.

Examples include:

Eid Campaign

New Year Sale

Flash Sale

Black Friday

Free Delivery Week

Customer Anniversary

Referral Campaign

Loyalty Rewards

Campaigns may automatically activate and expire based on scheduled dates.

---

# Analytics

Coupon data provides valuable business insights.

Examples include:

Most used coupons

Highest conversion coupons

Revenue generated through coupons

Average discount amount

Customer redemption rate

Shop coupon performance

Campaign effectiveness

Fraud detection reports

---

# Example Records

## Example 1

Coupon Code

WELCOME100

Coupon Type

platform

Discount Type

fixed

Discount Value

100.00

Minimum Order

1000.00

Status

active

---

## Example 2

Coupon Code

SHOP10

Coupon Type

shop

Shop ID

15

Discount Type

percentage

Discount Value

10

Maximum Discount

300.00

Status

active

---

# Future Expansion

Future versions may support:

Category-specific coupons

Product-specific coupons

First-order coupons

Birthday coupons

AI-generated personalized coupons

Geo-location based promotions

Referral reward automation

Stackable coupons

Coupon scheduling

Dynamic discount optimization

---

# Best Practices

Always validate coupons on the server.

Use UUIDs internally for tracking coupon events.

Prevent duplicate coupon redemption.

Maintain complete coupon usage history.

Archive expired coupons instead of permanently deleting them.

Monitor abnormal redemption activity.

Optimize coupon searches with proper indexing.

---

# Conclusion

The coupons table provides a secure, flexible, and scalable promotional infrastructure for HAT-BAZAR.

It supports:

Platform-wide promotions

Shop-specific discounts

Percentage and fixed discounts

Usage limits

Campaign management

Fraud prevention

Business analytics

Future AI-powered promotional strategies

This design enables efficient marketing campaigns while ensuring fairness, security, and long-term scalability across the marketplace ecosystem.

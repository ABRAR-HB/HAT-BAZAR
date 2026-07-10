# REVIEWS TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The reviews table stores customer feedback and ratings for purchased products.

The review system helps customers make informed purchasing decisions while rewarding honest sellers.

Only verified buyers can submit reviews.

Each completed order item can receive only one review.

---

# Table Name

reviews

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

The review system supports:

- Product ratings
- Written reviews
- Verified purchase badge
- Seller replies
- Review moderation
- Review reporting
- Review images
- Helpful votes
- Anti-spam protection

Reviews become part of the public product page after approval (if moderation is enabled).

---

# Ownership

Each review belongs to:

- One customer
- One product
- One order item

---

# Main Columns

id

user_id

product_id

shop_id

order_id

order_item_id

rating

title

review_text

verified_purchase

seller_reply

seller_reply_at

status

helpful_count

report_count

created_at

updated_at

deleted_at

---

# Review Rules

A customer can review only products they have successfully received.

Maximum one review per order_item.

Reviews cannot be transferred between products.

Deleted reviews remain in database through soft delete.

---

# Rating Scale

Allowed ratings

1

2

3

4

5

No decimal ratings.

Average ratings are calculated dynamically from all approved reviews.

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

Represents the customer who submitted the review.

---

## product_id

Type

BIGINT

Foreign Key

References

products(id)

Not Null

Indexed

---

## shop_id

Type

BIGINT

Foreign Key

References

shops(id)

Not Null

Indexed

---

## order_id

Type

BIGINT

Foreign Key

References

orders(id)

Not Null

Indexed

---

## order_item_id

Type

BIGINT

Foreign Key

References

order_items(id)

Unique

Not Null

Only one review per purchased item.

---

## rating

Type

SMALLINT

Not Null

Allowed Values

1

2

3

4

5

Database Constraint

CHECK (rating BETWEEN 1 AND 5)

---

## title

Type

VARCHAR(150)

Nullable

Short review headline.

---

## review_text

Type

TEXT

Nullable

Main customer review.

Maximum length may be controlled by application.

---

## verified_purchase

Type

BOOLEAN

Default TRUE

Automatically assigned after purchase verification.

Cannot be manually changed.

---

## seller_reply

Type

TEXT

Nullable

Seller may reply only once.

Replies are public.

---

## seller_reply_at

Type

TIMESTAMP

Nullable

Stores reply time.

---

## status

Type

VARCHAR(20)

Default

approved

Allowed Values

pending

approved

hidden

reported

rejected

deleted

---

## helpful_count

Type

INTEGER

Default 0

Number of users marking review as helpful.

---

## report_count

Type

INTEGER

Default 0

Number of abuse reports.

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

reviews.user_id

→ users.id

reviews.product_id

→ products.id

reviews.shop_id

→ shops.id

reviews.order_id

→ orders.id

reviews.order_item_id

→ order_items.id

---

# Constraints

One review per order item.

Only completed or delivered orders are reviewable.

Rating must be between 1 and 5.

Verified purchase cannot be manually modified.

Seller cannot edit customer review.

Customer cannot edit review after allowed edit period expires.

---

# Indexes

PRIMARY KEY (id)

INDEX (user_id)

INDEX (product_id)

INDEX (shop_id)

INDEX (order_id)

INDEX (status)

INDEX (rating)

INDEX (created_at)

Composite Index

(product_id, status)

(shop_id, rating)

(user_id, created_at)

---

# Seller Reply Rules

Seller may reply once.

Seller reply cannot modify customer review.

Seller reply becomes visible immediately after submission.

Reply timestamp must always be stored.

Reply history may be logged separately in audit logs.

---
# Helpful Vote System

Customers may mark reviews as helpful.

Rules

Only logged-in users may vote.

One helpful vote per user per review.

Users cannot vote on their own reviews.

Helpful votes increase review credibility but do not affect product rating.

---

# Review Report System

Customers may report inappropriate reviews.

Common report reasons

Spam

Fake Review

Offensive Language

Harassment

Irrelevant Content

Duplicate Review

Misleading Information

Reported reviews may enter moderation depending on report thresholds.

---

# Review Image Integration

Reviews may include product images.

Images are stored separately.

Reference Table

review_images

Each review may contain multiple images.

Supported Formats

JPG

PNG

WEBP

Maximum image size is controlled by application settings.

---

# Moderation Workflow

Status Flow

pending

↓

approved

↓

reported

↓

hidden

↓

rejected

Administrators can:

Approve reviews

Reject reviews

Hide reviews

Restore reviews

Delete reviews through soft delete

All moderation actions should be logged.

---

# Anti-Fake Review Rules

Only verified buyers may review.

Only delivered orders are eligible.

Duplicate reviews for the same order item are blocked.

Bulk review submissions should be rate limited.

AI-based fraud detection may flag suspicious reviews.

Suspicious patterns include:

Repeated identical text

Mass reviews from same device

Bot-generated content

Unusual rating behavior

---

# Rating Calculation

Product average rating is calculated using only:

Approved reviews

Visible reviews

Verified purchases (optional business rule)

Hidden or rejected reviews are excluded.

---

# Example Records

Example 1

Customer

ID 25

Product

ID 410

Rating

5

Title

Excellent Product

Verified Purchase

TRUE

Status

approved

---

Example 2

Customer

ID 91

Rating

2

Title

Packaging Damaged

Seller Reply

Sorry for the inconvenience. We will improve our packaging.

Status

approved

---

# Future Expansion

The review system supports future features such as:

Photo reviews

Video reviews

Review editing history

Verified seller responses

AI review summarization

Review translation

Review reactions

Review badges

Quality score

Reviewer reputation

Review analytics dashboard

---

# Security Notes

Customers cannot review products they never purchased.

Seller replies cannot modify original reviews.

Review ownership cannot be transferred.

All moderation actions must be logged.

Soft delete should be preferred over permanent deletion.

Rate limiting should protect against spam attacks.

---

# Conclusion

The reviews table provides a trustworthy product feedback system for HAT-BAZAR.

It ensures:

Verified customer reviews

Fair seller interaction

Reliable product ratings

Scalable moderation

Protection against fake reviews

Production-ready review management

This design supports transparency, customer trust, and long-term marketplace quality.

# SHOPS TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The shops table stores all registered shops on the HAT-BAZAR platform.

Every seller must own at least one verified shop before selling products.

One seller may own multiple shops in the future if platform policy allows.

---

# Table Name

shops

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Owner

owner_user_id

References:

users.id

One shop belongs to one seller.

---

# Core Columns

id

owner_user_id

shop_name

shop_slug

shop_description

shop_logo

shop_banner

shop_phone

division

district

upazila

union_name

village_or_area

postal_code

latitude

longitude

verification_status

shop_status

badge_level

followers_count

rating_average

total_reviews

total_products

total_orders

created_at

updated_at

deleted_at

---

# Verification Status

Possible values:

- pending
- under_review
- verified
- rejected

Only verified shops can publish products.

---

# Shop Status

Possible values:

- active
- inactive
- suspended
- closed

Only active shops are visible to customers.

---
---

# Column Specifications

| Column | Data Type | Required | Notes |
|---------|-----------|----------|-------|
| id | BIGSERIAL | Yes | Primary Key |
| owner_user_id | BIGINT | Yes | References users.id |
| shop_name | VARCHAR(200) | Yes | Shop display name |
| shop_slug | VARCHAR(250) | Yes | Unique shop URL |
| shop_description | TEXT | No | Shop description |
| shop_logo | TEXT | No | Logo image URL |
| shop_banner | TEXT | No | Banner image URL |
| shop_phone | VARCHAR(20) | Yes | Contact number |
| division | VARCHAR(100) | Yes | Division |
| district | VARCHAR(100) | Yes | District |
| upazila | VARCHAR(100) | Yes | Upazila |
| union_name | VARCHAR(100) | No | Union |
| village_or_area | VARCHAR(255) | Yes | Village / Area |
| postal_code | VARCHAR(20) | No | Postal Code |
| latitude | DECIMAL(10,7) | No | GPS Latitude |
| longitude | DECIMAL(10,7) | No | GPS Longitude |
| verification_status | VARCHAR(30) | Yes | Verification state |
| shop_status | VARCHAR(30) | Yes | Shop state |
| badge_level | SMALLINT | Yes | Default 4 |
| followers_count | INTEGER | Yes | Default 0 |
| rating_average | DECIMAL(3,2) | Yes | Default 0.00 |
| total_reviews | INTEGER | Yes | Default 0 |
| total_products | INTEGER | Yes | Default 0 |
| total_orders | INTEGER | Yes | Default 0 |
| created_at | TIMESTAMP | Yes | UTC |
| updated_at | TIMESTAMP | Yes | UTC |
| deleted_at | TIMESTAMP | No | Soft Delete |

---

# Unique Constraints

The following fields must be unique:

- shop_slug

---

# Indexes

Create indexes for:

- owner_user_id
- shop_name
- district
- verification_status
- shop_status
- created_at

---

# Relationships

This table connects with:

users.id

↓

shops.owner_user_id

Related Tables

- products
- orders
- reviews
- notifications
- badges

---

# Shop Address Rules

Every shop must provide:

- Division
- District
- Upazila
- Village / Area

GPS coordinates are optional but recommended.

They will support future rider navigation and map services.

---

# Shop Approval Workflow

Seller submits shop

↓

Documents verified

↓

Admin review

↓

Approved

↓

Verified Shop

↓

Seller can publish products

Rejected shops cannot publish products.

---
---

# Follow System

Customers may follow shops.

Following a shop allows customers to receive notifications for:

- New products
- Product restocks
- Special offers
- Flash sales
- Important shop announcements

The actual follower relationship is maintained in a separate table.

followers_count stores only the aggregated value for fast retrieval.

---

# SEO Policy

Every shop has a permanent unique slug.

Example:

https://hat-bazar.com/shop/rahman-electronics

The slug should:

- Be unique
- Contain lowercase letters
- Use hyphens instead of spaces
- Never change after publication unless approved by an administrator

---

# Business Rules

A seller cannot publish products until the shop is verified.

A suspended or closed shop cannot:

- Publish new products
- Receive new orders
- Participate in campaigns

Customers can still view historical order information even if a shop is closed.

---

# Future Expansion

The database structure supports future features without breaking compatibility.

Future planned features:

- Multiple shops per seller
- Shop managers with role-based permissions
- Premium shop subscriptions
- Advanced shop analytics
- Multi-branch shops
- Franchise management

These features remain disabled until officially introduced.

---

# Security Notes

The shop owner can edit only their own shop.

Administrative verification actions must be logged.

Every important shop action should create an audit log.

Shop verification documents should be stored securely and must never be publicly accessible.

---

# Summary

The shops table is the central business entity of the HAT-BAZAR marketplace.

Every product belongs to a shop.

Every shop belongs to a seller.

The shops table connects users, products, orders, badges, reviews, notifications, and future business modules.

---

# PRODUCTS TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The products table stores every product that can be sold on the HAT-BAZAR platform.

Every product belongs to exactly one shop.

A product cannot exist without an approved shop.

---

# Table Name

products

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Ownership

shop_id

References:

shops.id

Each product belongs to one shop.

---

# Core Columns

id

shop_id

category_id

product_name

product_slug

short_description

full_description

brand_name

sku

barcode

unit

regular_price

sale_price

cost_price

currency

stock_quantity

minimum_stock_level

maximum_order_quantity

product_status

approval_status

is_featured

is_digital

total_views

total_sales

rating_average

total_reviews

created_at

updated_at

deleted_at

---

# Product Status

Possible values:

- draft
- active
- out_of_stock
- hidden
- discontinued

Only active products are visible to customers.

---

# Approval Status

Possible values:

- pending
- approved
- rejected

Products requiring moderation cannot be published until approved.

---
---

# Column Specifications

| Column | Data Type | Required | Notes |
|---------|-----------|----------|-------|
| id | BIGSERIAL | Yes | Primary Key |
| shop_id | BIGINT | Yes | References shops.id |
| category_id | BIGINT | Yes | References categories.id |
| product_name | VARCHAR(255) | Yes | Product name |
| product_slug | VARCHAR(300) | Yes | Unique SEO URL |
| short_description | VARCHAR(500) | Yes | Short summary |
| full_description | TEXT | Yes | Complete description |
| brand_name | VARCHAR(150) | No | Brand |
| sku | VARCHAR(100) | Yes | Stock Keeping Unit |
| barcode | VARCHAR(100) | No | Optional barcode |
| unit | VARCHAR(50) | Yes | Piece, Kg, Litre, Dozen etc. |
| regular_price | DECIMAL(12,2) | Yes | Original selling price |
| sale_price | DECIMAL(12,2) | No | Discounted price |
| cost_price | DECIMAL(12,2) | No | Seller/Admin only |
| currency | VARCHAR(10) | Yes | Default: BDT |
| stock_quantity | INTEGER | Yes | Current stock |
| minimum_stock_level | INTEGER | Yes | Low stock warning |
| maximum_order_quantity | INTEGER | Yes | Purchase limit |
| product_status | VARCHAR(30) | Yes | Product state |
| approval_status | VARCHAR(30) | Yes | Approval state |
| is_featured | BOOLEAN | Yes | Default FALSE |
| is_digital | BOOLEAN | Yes | Default FALSE |
| total_views | BIGINT | Yes | Default 0 |
| total_sales | BIGINT | Yes | Default 0 |
| rating_average | DECIMAL(3,2) | Yes | Default 0.00 |
| total_reviews | INTEGER | Yes | Default 0 |
| created_at | TIMESTAMP | Yes | UTC |
| updated_at | TIMESTAMP | Yes | UTC |
| deleted_at | TIMESTAMP | No | Soft Delete |

---

# Unique Constraints

The following fields must be unique within a shop:

- sku
- product_slug

---

# Indexes

Create indexes for:

- shop_id
- category_id
- product_name
- product_status
- approval_status
- created_at

---

# Pricing Rules

regular_price must always be greater than zero.

sale_price:

- Cannot be greater than regular_price.
- May be NULL when no discount exists.

cost_price:

- Never visible to customers.
- Visible only to authorized seller and administrators.

---

# Stock Rules

Stock cannot become negative.

When stock reaches zero:

- Product automatically becomes Out of Stock.

When stock reaches minimum_stock_level:

- Seller receives a low-stock notification.

maximum_order_quantity prevents bulk abuse and accidental purchases.

---

# Product Visibility

Customers can only view products when:

- Shop is Active
- Shop is Verified
- Product Status = Active
- Approval Status = Approved

Otherwise, the product remains hidden from public search.

---
---

# Product Images

Product images are stored in a separate table.

Table:

product_images

Each product may have multiple images.

Fields include:

- id
- product_id
- image_url
- display_order
- is_primary
- created_at

The primary image is displayed throughout the platform.

---

# Product Variants

Variants are stored separately.

Table:

product_variants

Supported examples:

- Color
- Size
- Weight
- Capacity
- Material
- Storage
- RAM

Each variant may have:

- Individual SKU
- Individual Price
- Individual Stock
- Individual Barcode

This design supports unlimited variants.

---

# Product Search

Products should support searching by:

- Product Name
- Brand
- Category
- SKU
- Tags
- Description

Future AI-powered semantic search can be added without changing the core products table.

---

# SEO Policy

Each product has a permanent unique slug.

Example:

https://hat-bazar.com/product/samsung-galaxy-a56

Rules:

- Lowercase only
- Hyphen-separated words
- Unique across all products
- Should not change after publication unless approved

---

# Business Rules

A product cannot exist without a shop.

A deleted shop cannot create new products.

A hidden product:

- Cannot appear in search
- Cannot be purchased
- Remains available in historical orders

Products that have already been purchased should never be permanently removed from historical order records.

---

# Future Expansion

The products table is designed for future scalability.

Future features include:

- AI product recommendations
- Related products
- Product bundles
- Subscription products
- Digital products
- Scheduled publishing
- Advanced inventory management
- Warehouse support

These can be implemented without changing the core table structure.

---

# Security Notes

Only the shop owner or authorized administrators may create, edit, or archive products.

Every important product action should be recorded in the audit log.

Product prices, stock, and approval actions should always be traceable.

---

# Summary

The products table is the core inventory table of the HAT-BAZAR platform.

Every product belongs to one shop and one category.

The table integrates with orders, payments, search, reviews, notifications, inventory, and future AI services while remaining scalable for long-term growth.

---

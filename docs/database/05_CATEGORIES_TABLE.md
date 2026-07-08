# CATEGORIES TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The categories table organizes all products into a structured hierarchy.

Every product belongs to one category.

Categories support unlimited nested subcategories for future platform growth.

---

# Table Name

categories

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Parent Category

parent_category_id

References:

categories.id

NULL means this is a root category.

This structure supports unlimited category depth.

---

# Core Columns

id

parent_category_id

category_name

category_slug

category_description

category_icon

category_banner

display_order

category_level

is_featured

is_active

total_products

meta_title

meta_description

created_at

updated_at

deleted_at

---

# Category Types

Supported hierarchy example:

Electronics

↓

Mobile Phones

↓

Android Phones

↓

Samsung

↓

Galaxy Series

There is no fixed depth limit.

---

# Category Status

Possible values:

- active
- inactive

Inactive categories are hidden from customers.

---
---

# Column Specifications

| Column | Data Type | Required | Notes |
|---------|-----------|----------|-------|
| id | BIGSERIAL | Yes | Primary Key |
| parent_category_id | BIGINT | No | References categories.id |
| category_name | VARCHAR(200) | Yes | Category name |
| category_slug | VARCHAR(250) | Yes | Unique SEO slug |
| category_description | TEXT | No | Description |
| category_icon | TEXT | No | Icon URL |
| category_banner | TEXT | No | Banner image URL |
| display_order | INTEGER | Yes | Default 0 |
| category_level | SMALLINT | Yes | Root = 1 |
| is_featured | BOOLEAN | Yes | Default FALSE |
| is_active | BOOLEAN | Yes | Default TRUE |
| total_products | INTEGER | Yes | Default 0 |
| meta_title | VARCHAR(255) | No | SEO title |
| meta_description | TEXT | No | SEO description |
| created_at | TIMESTAMP | Yes | UTC |
| updated_at | TIMESTAMP | Yes | UTC |
| deleted_at | TIMESTAMP | No | Soft Delete |

---

# Unique Constraints

The following fields must be unique:

- category_slug

Within the same parent category:

- category_name must be unique

---

# Indexes

Create indexes for:

- parent_category_id
- category_name
- category_slug
- display_order
- is_active
- created_at

---

# Relationships

The categories table connects with:

- products.category_id
- parent_category_id → categories.id

This creates a self-referencing tree structure.

---

# Category Rules

Root categories have:

parent_category_id = NULL

Subcategories reference their parent.

Categories may contain unlimited child categories.

A parent category may contain:

- Products
- Subcategories
- Both

---

# Display Rules

Only active categories appear:

- Homepage
- Navigation Menu
- Search Filters
- Product Creation

Inactive categories remain in the database but are hidden from users.

---

# Product Assignment Rules

Each product belongs to one category.

Products should preferably be assigned to the lowest (leaf) category to improve search accuracy and filtering.

---

---

# SEO Policy

Each category has a permanent unique slug.

Example:

https://hat-bazar.com/category/electronics

Rules:

- Lowercase only
- Hyphen-separated words
- Unique across all categories
- Should not change after publication unless approved by an administrator

---

# Business Rules

A category cannot be permanently deleted if products are assigned to it.

Instead:

- Mark the category as inactive.
- Move products to another category before deletion if necessary.

Deleting a parent category must never automatically delete its child categories.

---

# Category Navigation

Categories support:

- Homepage menus
- Mega Menu
- Product Filters
- Breadcrumb Navigation
- Search Suggestions

The hierarchy should be generated dynamically using parent_category_id.

---

# Future Expansion

The categories table supports future platform growth.

Planned features include:

- Category Attributes
- Category Filters
- AI Product Classification
- Seasonal Categories
- Featured Collections
- Regional Categories
- Dynamic Navigation Menus
- Multi-language Category Names

These features can be added without changing the core table structure.

---

# Security Notes

Only authorized administrators may:

- Create categories
- Edit categories
- Delete categories
- Change hierarchy
- Change display order

All administrative actions must be recorded in the audit log.

---

# Summary

The categories table provides the product classification system for HAT-BAZAR.

Its self-referencing design allows unlimited nested categories while maintaining high performance and future scalability.

It integrates with products, search, navigation, recommendations, filters, and future AI-powered services.

---

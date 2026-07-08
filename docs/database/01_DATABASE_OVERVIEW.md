# HAT-BAZAR DATABASE OVERVIEW

Version: 1.0

Status: Official Database Design

---

# Purpose

This document defines the database architecture of the HAT-BAZAR platform.

It establishes the standards, naming conventions, relationships, constraints, and design principles that every database table must follow.

All future database tables must comply with this document.

---

# Database Engine

Production Database

- PostgreSQL

Development Database

- PostgreSQL

Reason:

- High Performance
- ACID Compliance
- Excellent Indexing
- JSON Support
- Full Text Search
- Scalability
- Enterprise Ready

---

# Database Design Principles

The HAT-BAZAR database follows these principles:

- Normalized Design
- High Performance
- Data Integrity
- Scalability
- Security
- Maintainability
- Future Expansion

---

# Naming Convention

## Tables

Use:

snake_case

Examples

users

shops

products

orders

wallet_transactions

notification_logs

---

## Columns

Use:

snake_case

Examples

first_name

phone_number

created_at

updated_at

seller_level

wallet_balance

---

## Primary Key

Every table uses

id

BIGINT

Auto Increment

---

## Foreign Keys

Always use:

user_id

shop_id

product_id

order_id

payment_id

review_id

Never use inconsistent naming.
---

# Standard Audit Fields

Every business table must contain the following fields.

created_at

updated_at

created_by

updated_by

These fields help track who created and modified records.

---

# Soft Delete Policy

Business data should normally use Soft Delete.

Standard field:

deleted_at

Rules:

- Deleted records remain in the database.
- Normal queries ignore deleted records.
- Admins may restore deleted records if permitted.

Permanent deletion should only be performed by authorized administrators or scheduled cleanup jobs.

---

# Timestamp Policy

All timestamps must use UTC.

Frontend applications are responsible for displaying the user's local timezone.

Standard timestamp fields:

created_at

updated_at

deleted_at

---

# Status Fields

Instead of storing Yes/No values where a lifecycle exists, use descriptive status fields.

Examples:

User

- active
- inactive
- suspended

Shop

- pending
- verified
- suspended
- closed

Order

- pending
- confirmed
- packed
- shipped
- delivered
- cancelled
- returned
- refunded

Payment

- pending
- successful
- failed
- refunded

---

# Index Strategy

Indexes should be created on frequently queried columns.

Examples:

phone_number

email

shop_id

product_id

category_id

order_id

seller_id

customer_id

created_at

Composite indexes should be added only when supported by query analysis.

---

# Constraints

Every table should enforce data integrity.

Examples:

- NOT NULL where required
- UNIQUE on phone numbers
- UNIQUE on email addresses (when applicable)
- Foreign Key Constraints
- CHECK Constraints where appropriate

---

# Security Rules

Sensitive information must never be stored in plain text.

Examples:

- Passwords → Hashed
- OTP → Temporary and Expiring
- NID → Encrypted
- Payment Tokens → Encrypted

The database should never store confidential payment credentials.

---

# Backup Strategy

Production database should support:

- Daily Full Backup
- Hourly Incremental Backup
- Point-in-Time Recovery
- Automated Backup Verification

Backups should be stored securely in separate storage.

---

# Database Documentation Policy

Every new table must have its own documentation including:

- Purpose
- Columns
- Data Types
- Constraints
- Relationships
- Indexes
- Notes

This ensures long-term maintainability and consistency.

---

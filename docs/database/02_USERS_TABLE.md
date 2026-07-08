# USERS TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The users table is the central identity table of the HAT-BAZAR platform.

Every authenticated person has exactly one user record.

Examples:

- Customer
- Seller
- Rider
- Admin
- Super Admin
- Future Shop Manager

Other profile tables extend this table.

---

# Table Name

users

---

# Primary Key

id

BIGINT

Auto Increment

---

# Core Columns

id

first_name

last_name

full_name

phone_number

email

password_hash

google_id

profile_photo

date_of_birth

gender

preferred_language

country

status

role

email_verified

phone_verified

last_login_at

created_at

updated_at

deleted_at

---

# Role Values

Supported roles:

- customer
- seller
- rider
- admin
- super_admin

Future:

- shop_manager

A user always has one primary role.

Additional permissions may be assigned through the authorization system.

---

# Status Values

Possible values:

- pending
- active
- suspended
- blocked
- deleted

Only active users may access protected platform features.
---

# Column Specifications

| Column | Data Type | Required | Notes |
|---------|-----------|----------|-------|
| id | BIGSERIAL | Yes | Primary Key |
| first_name | VARCHAR(100) | Yes | User first name |
| last_name | VARCHAR(100) | No | User last name |
| full_name | VARCHAR(200) | Yes | Generated or stored full name |
| phone_number | VARCHAR(20) | Yes | Must be unique |
| email | VARCHAR(255) | No | Unique if provided |
| password_hash | TEXT | Yes | Secure hashed password |
| google_id | VARCHAR(255) | No | Google Login ID |
| profile_photo | TEXT | No | Image URL |
| date_of_birth | DATE | No | Optional |
| gender | VARCHAR(20) | No | Male, Female, Other |
| preferred_language | VARCHAR(20) | Yes | Default: Bengali |
| country | VARCHAR(100) | Yes | Default: Bangladesh |
| role | VARCHAR(30) | Yes | Primary role |
| status | VARCHAR(30) | Yes | User status |
| email_verified | BOOLEAN | Yes | Default FALSE |
| phone_verified | BOOLEAN | Yes | Default FALSE |
| last_login_at | TIMESTAMP | No | Last login time |
| created_at | TIMESTAMP | Yes | UTC |
| updated_at | TIMESTAMP | Yes | UTC |
| deleted_at | TIMESTAMP | No | Soft delete |

---

# Unique Constraints

The following fields must be unique:

- phone_number
- email (when not NULL)
- google_id (when not NULL)

---

# Indexes

Indexes should be created for:

- phone_number
- email
- role
- status
- created_at

These indexes improve authentication and user search performance.

---

# Relationships

The users table is referenced by multiple business tables.

Examples:

- shops.user_id
- orders.customer_id
- seller_profiles.user_id
- rider_profiles.user_id
- wallets.user_id
- notifications.user_id
- chats.sender_id
- chats.receiver_id

---

# Validation Rules

Phone Number

- Must follow Bangladesh mobile format.
- Digits only after normalization.

Email

- Must follow valid email format.

Password

- Never stored in plain text.
- Store only hashed values.

---
---

# Authentication Policy

A user may log in using:

- Phone Number + Password
- Google Account

Future support:

- Apple Sign In
- Passkeys

Only verified accounts may access protected platform services.

---

# Account Lifecycle

Every user account follows this lifecycle:

Pending Registration

↓

Phone Verification

↓

Active

↓

Suspended (if policy violation)

↓

Blocked (serious violation)

↓

Soft Deleted

↓

Permanent Removal (Admin Only)

---

# Security Notes

The users table must never store:

- Plain text passwords
- OTP history permanently
- Payment credentials
- National ID in plain text

Sensitive information must always be encrypted or securely hashed.

---

# Future Expansion

The users table is designed to support future platform growth.

Future features include:

- Multi-role support
- Shop Manager accounts
- Multiple verified identities
- Multi-country deployment
- AI identity verification
- Government digital identity integration

These features should be implemented without breaking the existing database structure.

---

# Summary

The users table is the central identity system of the HAT-BAZAR platform.

All authenticated platform users originate from this table.

Every authentication, authorization, wallet, order, notification, shop, and rider module ultimately references this table.

---

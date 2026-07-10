# ADDRESSES TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The addresses table stores delivery and billing addresses for HAT-BAZAR users.

A user may save multiple addresses such as Home, Office, Business, or Other.

These addresses are used during checkout, delivery, returns, refunds, and future subscription services.

---

# Table Name

addresses

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Each address belongs to exactly one user.

Users can save multiple addresses but only one address of each type can be marked as default.

Orders always store a snapshot of the selected address to preserve historical accuracy.

Future changes to an address must never modify past order records.

---

# Ownership

Each address belongs to:

- One user

One user can own many addresses.

---

# Main Columns

id

user_id

label

recipient_name

phone_number

country

division

district

upazila

union_name

area

postal_code

address_line_1

address_line_2

landmark

latitude

longitude

is_default

address_type

status

created_at

updated_at

deleted_at

---

# Supported Address Types

Home

Office

Business

Other

---

# Business Rules

Every address belongs to one registered user.

Users may create multiple addresses.

Only one default address is allowed per address type.

Soft delete must be used.

Orders must never reference a modified address directly.

Instead, order delivery information should be copied into the order at checkout.

---

# Address Usage

Addresses are used by:

Checkout

Order Delivery

Return Pickup

Refund Pickup

Seller Returns

Future Subscription Orders

Future Scheduled Deliveries

---

# Location Support

The system is designed primarily for Bangladesh.

Administrative hierarchy

Country

↓

Division

↓

District

↓

Upazila

↓

Union (optional)

↓

Area / Village

↓

Street Address

Future international expansion is supported.

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

Owner of the address.

---

## label

Type

VARCHAR(50)

Nullable

Examples

Home

Office

Village House

Factory

Apartment

---

## recipient_name

Type

VARCHAR(150)

Not Null

Person who will receive the order.

---

## phone_number

Type

VARCHAR(20)

Not Null

Delivery contact number.

---

## country

Type

VARCHAR(100)

Default

Bangladesh

---

## division

Type

VARCHAR(100)

Not Null

---

## district

Type

VARCHAR(100)

Not Null

---

## upazila

Type

VARCHAR(100)

Not Null

---

## union_name

Type

VARCHAR(100)

Nullable

Optional administrative area.

---

## area

Type

VARCHAR(150)

Not Null

Neighborhood, village, or locality.

---

## postal_code

Type

VARCHAR(20)

Nullable

---

## address_line_1

Type

VARCHAR(255)

Not Null

Primary street address.

---

## address_line_2

Type

VARCHAR(255)

Nullable

Apartment, floor, building, etc.

---

## landmark

Type

VARCHAR(255)

Nullable

Nearby identifiable location.

---

## latitude

Type

DECIMAL(10,8)

Nullable

GPS latitude for map-based delivery.

---

## longitude

Type

DECIMAL(11,8)

Nullable

GPS longitude for map-based delivery.

---

## is_default

Type

BOOLEAN

Default

FALSE

---

## address_type

Type

VARCHAR(30)

Not Null

Allowed Values

home

office

business

other

---

## status

Type

VARCHAR(20)

Default

active

Allowed Values

active

inactive

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

addresses.user_id

→ users.id

---

# Constraints

Each address must belong to one user.

Recipient name is mandatory.

Phone number is mandatory.

Only one default address is allowed for each address type.

Latitude and longitude must both be present if GPS coordinates are stored.

Deleted addresses cannot be selected during checkout.

---

# Indexes

PRIMARY KEY (id)

INDEX (user_id)

INDEX (status)

INDEX (address_type)

INDEX (postal_code)

Composite Index

(user_id, is_default)

(user_id, address_type)

---

# Default Address Rules

A user may save multiple addresses.

Changing the default address automatically removes the default flag from the previous address of the same type.

Only active addresses may become default.

---

# Delivery Validation

Before checkout the system verifies:

Address exists

Address belongs to the user

Address is active

Delivery service is available for the selected location

---

# Privacy & Security

Users may access only their own addresses.

Address APIs require authentication.

GPS coordinates should never be exposed publicly.

Deleted addresses remain available for audit purposes but are hidden from users.

# Order Snapshot Integration

Orders must never directly depend on the addresses table.

During checkout, the selected delivery address is copied into the order.

The snapshot should include:

Recipient Name

Phone Number

Country

Division

District

Upazila

Union

Area

Postal Code

Address Line 1

Address Line 2

Landmark

Latitude

Longitude

This preserves historical delivery information even if the customer edits or deletes the original address later.

---

# Rider Integration

Riders use the delivery address stored in the order snapshot.

The rider should never rely on the customer's current address record.

GPS coordinates can be used for:

Navigation

Route optimization

Estimated arrival time

Delivery confirmation

Future live tracking

---

# Return & Refund Integration

Addresses may also be used for:

Product return pickup

Refund pickup

Warranty service

Exchange requests

Customers may choose either:

Original delivery address

Another saved address

The selected address is copied into the return request.

---

# Example Records

## Example 1

User ID

25

Label

Home

Recipient

Abrar Hossain

Phone

+8801712345678

Division

Dhaka

District

Dhaka

Upazila

Dhanmondi

Area

Road 27

Address

House 15, Flat 4B

Default

Yes

Status

active

---

## Example 2

User ID

25

Label

Office

Recipient

Abrar Hossain

Phone

+8801812345678

Division

Dhaka

District

Dhaka

Upazila

Tejgaon

Area

Karwan Bazar

Address

Building 22, Floor 8

Default

No

Status

active

---

# Future Expansion

The address system supports future features including:

Google Maps integration

OpenStreetMap integration

Location autocomplete

Address verification

Delivery zone detection

Distance calculation

Delivery charge estimation

Favorite addresses

Recently used addresses

International shipping

---

# Best Practices

Always use soft delete.

Never overwrite historical order addresses.

Validate ownership before every operation.

Store GPS coordinates only when available.

Normalize administrative area names.

Prevent duplicate default addresses.

Keep address validation in the application layer.

---

# Conclusion

The addresses table provides a secure, scalable, and production-ready address management system for HAT-BAZAR.

It supports:

Multiple addresses per user

Default address management

GPS-based delivery

Order address snapshots

Return and refund operations

Future international expansion

This design ensures accurate deliveries while preserving historical order information and maintaining data integrity across the marketplace.	

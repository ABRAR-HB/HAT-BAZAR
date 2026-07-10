# REVIEW IMAGES TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The review_images table stores images uploaded by customers along with their product reviews.

Separating images into a dedicated table keeps the database normalized and allows each review to contain multiple images.

This design also prepares the platform for future support of videos, AI image moderation, and image analytics.

---

# Table Name

review_images

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Each image belongs to exactly one review.

A review can have multiple images.

Images are stored in object storage (such as AWS S3, Cloudflare R2, or MinIO), while this table stores only metadata.

---

# Ownership

Each review image belongs to:

- One review
- One customer
- One product

---

# Main Columns

id

review_id

user_id

product_id

image_url

thumbnail_url

storage_provider

file_name

mime_type

file_size

width

height

display_order

status

uploaded_at

created_at

updated_at

deleted_at

---

# Business Rules

Only the review owner can upload review images.

Images cannot exist without a review.

Deleting a review does not immediately remove images from storage.

Images should be removed later through scheduled cleanup jobs.

Maximum images per review should be controlled by application settings.

Recommended default:

Maximum 10 images per review.

---
# Column Definitions

---

## id

Type

BIGSERIAL

Primary Key

Not Null

---

## review_id

Type

BIGINT

Foreign Key

References

reviews(id)

Not Null

Indexed

Each image belongs to one review.

---

## user_id

Type

BIGINT

Foreign Key

References

users(id)

Not Null

Indexed

Stores the image owner.

---

## product_id

Type

BIGINT

Foreign Key

References

products(id)

Not Null

Indexed

Used for faster product review image retrieval.

---

## image_url

Type

TEXT

Not Null

Stores the original image location.

Example

https://cdn.hatbazar.com/reviews/2026/07/image001.webp

---

## thumbnail_url

Type

TEXT

Nullable

Stores optimized thumbnail image.

Used for fast page loading.

---

## storage_provider

Type

VARCHAR(30)

Default

Cloudflare R2

Supported Values

Cloudflare R2

AWS S3

MinIO

Local Storage

Future providers

---

## file_name

Type

VARCHAR(255)

Not Null

Original uploaded filename.

---

## mime_type

Type

VARCHAR(50)

Not Null

Examples

image/jpeg

image/png

image/webp

---

## file_size

Type

BIGINT

Not Null

Unit

Bytes

Maximum size controlled by application.

---

## width

Type

INTEGER

Nullable

Image width in pixels.

---

## height

Type

INTEGER

Nullable

Image height in pixels.

---

## display_order

Type

SMALLINT

Default 1

Controls image display sequence.

---

## status

Type

VARCHAR(20)

Default

active

Allowed Values

active

hidden

processing

rejected

deleted

---

## uploaded_at

TIMESTAMP

Default CURRENT_TIMESTAMP

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

review_images.review_id

→ reviews.id

review_images.user_id

→ users.id

review_images.product_id

→ products.id

---

# Constraints

Image must belong to an existing review.

Only review owner may upload images.

Image URLs cannot be empty.

Display order must be positive.

Deleted reviews should not expose their images publicly.

---

# Indexes

PRIMARY KEY (id)

INDEX (review_id)

INDEX (user_id)

INDEX (product_id)

INDEX (status)

INDEX (uploaded_at)

Composite Index

(review_id, display_order)

(product_id, status)

(user_id, uploaded_at)

---

# Supported Image Formats

JPEG

PNG

WEBP

Future Support

HEIC

AVIF

GIF (optional)

Unsupported formats should be rejected before upload.

---
# Image Upload Flow

Step 1

Customer submits a review.

↓

Step 2

Customer uploads one or more images.

↓

Step 3

Application validates:

File type

File size

Image dimensions

Malware scan

↓

Step 4

Image is uploaded to object storage.

↓

Step 5

Thumbnail is generated.

↓

Step 6

Metadata is stored in review_images table.

↓

Step 7

Image becomes visible with the approved review.

---

# Image Moderation

Images may be reviewed automatically or manually.

Possible moderation actions:

Approve

Hide

Reject

Delete (Soft Delete)

Reasons may include:

Adult content

Violence

Copyright violation

Spam

Irrelevant image

Fake image

All moderation actions should be recorded in audit logs.

---

# AI Image Analysis

Future AI services may analyze uploaded images for:

Product matching

Duplicate image detection

NSFW detection

Blur detection

Low-quality image detection

Fake image detection

Brand/logo recognition

These analyses should not modify the original uploaded file.

---

# CDN & Cache Strategy

Images should be delivered through a CDN.

Benefits:

Lower latency

Reduced server load

Global availability

Fast thumbnail loading

Cache invalidation should occur automatically when an image is replaced or hidden.

---

# Security Rules

Only authenticated users may upload images.

Only review owners may modify or delete their images.

Direct public write access to storage must be disabled.

Signed upload URLs are recommended.

All uploaded filenames should be randomized to prevent guessing.

Metadata should never expose internal storage paths.

---

# Example Records

Example 1

Review ID

520

Image URL

reviews/2026/07/review_520_img1.webp

Thumbnail

reviews/thumbs/review_520_img1.webp

Status

active

Display Order

1

---

Example 2

Review ID

520

Image URL

reviews/2026/07/review_520_img2.webp

Status

active

Display Order

2

---

# Future Expansion

The review_images table is designed to support:

Review videos

360° product images

AI-generated thumbnails

Automatic image compression

Image watermarking

Multi-resolution storage

Image version history

Content moderation scoring

Image search

Visual similarity detection

---

# Best Practices

Never store image binary data inside the database.

Store only metadata and object storage URLs.

Generate thumbnails asynchronously.

Compress images before storage.

Use immutable image URLs whenever possible.

Maintain soft delete for audit purposes.

---

# Conclusion

The review_images table provides a scalable and production-ready solution for managing review media in HAT-BAZAR.

It ensures:

Multiple images per review

Secure media storage

Fast content delivery

Future AI integration

Efficient moderation

High scalability for marketplace growth

This design follows modern cloud-native best practices and keeps the database lightweight while supporting rich customer reviews.

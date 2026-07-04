# HAT-BAZAR Product System

**Document ID:** HB-PRODUCT-001
**Status:** Final
**Version:** 1.0
**Last Updated:** 2026-07-04

---

# Purpose

HAT-BAZAR Product System এমনভাবে তৈরি করা হয়েছে যাতে Seller সহজে Product পরিচালনা করতে পারে এবং Customer সঠিক ও নির্ভরযোগ্য Product তথ্য পায়।

---

# Product Information

Seller নতুন Product যোগ করার সময় নিচের তথ্যগুলো প্রদান করবে।

## Basic Information

- Product Name
- Category
- Brand (Optional)
- Short Description
- Full Description

---

## Price Information

- Regular Price
- Discount Price (Optional)

---

## Stock Information

- Available Quantity

---

## Media

### Images

- Minimum 1 Image
- Maximum 10 Images

### Video

- Maximum 1 Product Video (Optional)

---

## Delivery

- Product Weight (যদি প্রয়োজন হয়)

---

# Business Rules

- Product Name খালি রাখা যাবে না।
- Price অবশ্যই ০ টাকার বেশি হবে।
- Stock Negative হতে পারবে না।
- কমপক্ষে ১টি Image ছাড়া Product Publish করা যাবে না।

---

# Product Media

Seller Product-এর জন্য Image এবং Video আপলোড করতে পারবে।

---

## AI Media Verification

AI পরীক্ষা করবে:

- Image ঝাপসা কি না।
- Video Product-এর সাথে মিলছে কি না।
- Duplicate Media আছে কি না।
- নিষিদ্ধ বা অপ্রাসঙ্গিক Content আছে কি না।

---

## Business Rules

- প্রথম Image Cover Image হবে।
- Seller Cover Image পরিবর্তন করতে পারবে।
- Video Optional হবে।
- অন্য Shop-এর Media অনুমতি ছাড়া ব্যবহার করা যাবে না।
- সন্দেহজনক Media হলে Product Review-এ যাবে।

---

# Product Status

Product-এর Status হবে:

- Active
- Draft
- Out of Stock
- Under Review
- Suspended

---

## Business Rules

- Seller Draft হিসেবে Save করতে পারবে।
- Stock 0 হলে Out of Stock হবে।
- Stock যোগ হলে আবার Active হবে।
- Admin Product Suspend করতে পারবে।
- AI/Admin Review চললে Under Review দেখাবে।

---

# Product Edit

Seller Edit করতে পারবে:

- Product Name
- Description
- Price
- Discount
- Stock
- Images
- Video
- Brand
- Category

---

## Business Rules

- পরিবর্তন সঙ্গে সঙ্গে Save হবে।
- বড় পরিবর্তনে AI/Admin Review হতে পারে।
- অন্য Seller-এর Product Edit করা যাবে না।

---

# Product Delete

Seller Permanent Delete করতে পারবে না।

Seller পারবে:

- Archive
- Inactive
- Restore

Admin বিশেষ প্রয়োজনে Permanent Delete করতে পারবে।

---

## Business Rules

- Order History থাকা Product Permanent Delete করা যাবে না।
- Archive Product Restore করা যাবে।
- Order History সংরক্ষিত থাকবে।

---

# AI Product Verification

AI পরীক্ষা করবে:

- Product Image
- Product Video
- Description
- Price
- Duplicate Product
- Prohibited Product
- Copyright Content

---

## Business Rules

- সব নতুন Product AI পরীক্ষা করবে।
- সন্দেহজনক Product Under Review হবে।
- Admin চূড়ান্ত সিদ্ধান্ত নেবে।
- AI নিজে থেকে Product Delete করবে না।

---

# Product Stock Management

Stock সম্পূর্ণ Automatic হবে।

---

## Business Rules

- সফল Order-এর পর Stock কমবে।
- Cancel হলে Stock ফেরত আসবে।
- Return সম্পন্ন হলে Stock যোগ হবে।
- Stock 0 হলে Out of Stock হবে।
- Seller Stock Update করতে পারবে।
- Stock Negative হবে না।
- Stock History সংরক্ষিত হবে।

---

# Future Scope

- AI Product Description Generator
- AI Product Tag Generator
- Multi-language Product Description
- 3D Product View

---

# Final Decision

- Product Information থাকবে।
- Image ও Video থাকবে।
- Product Status থাকবে।
- Product Edit থাকবে।
- Product Archive থাকবে।
- AI Verification থাকবে।
- Automatic Stock Management থাকবে।

---

Approved by:

HAT-BAZAR Blueprint Team

Version 1.0

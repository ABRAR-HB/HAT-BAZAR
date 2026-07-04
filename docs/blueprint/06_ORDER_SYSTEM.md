# HAT-BAZAR Order System

**Document ID:** HB-ORDER-001
**Status:** Final
**Version:** 1.0
**Last Updated:** 2026-07-04

---

# Purpose

HAT-BAZAR-এর Order System এমনভাবে তৈরি করা হয়েছে যাতে Customer, Seller, Rider এবং HAT-BAZAR-এর মধ্যে প্রতিটি Order নিরাপদ, স্বচ্ছ এবং সহজভাবে পরিচালিত হয়।

---

# Order Flow

1. Customer Cart-এ Product যোগ করবে।
2. Checkout করবে।
3. Online Payment করবে।
4. Payment সফল হলে Order Confirm হবে।
5. Backend প্রতিটি Shop-এর জন্য আলাদা Order তৈরি করবে।
6. Seller Order Accept বা Reject করবে।
7. Admin Rider Assign করবে।
8. Rider Product Pickup করবে।
9. Rider Customer-এর কাছে Product Delivery করবে।
10. Delivery-এর পর ৩ দিনের Return Period শুরু হবে।
11. Return না হলে Seller-এর Wallet-এ Payment Release হবে।

---

# Multi Shop Order

Customer একাধিক Shop থেকে Product কিনতে পারবে।

কিন্তু Backend প্রতিটি Shop-এর জন্য আলাদা Order তৈরি করবে।

---

# Customer Cancel Rule

Customer Rider Pickup শুরু হওয়ার আগ পর্যন্ত Order Cancel করতে পারবে।

Pickup শুরু হলে Cancel Option বন্ধ হয়ে যাবে।

---

# Seller Response

Seller Order Accept অথবা Reject করতে পারবে।

Reject করলে কারণ উল্লেখ করতে হবে।

Customer-এর টাকা একই Payment Method-এ Refund হবে।

---

# Seller Response Time

Seller ২৪ ঘণ্টার মধ্যে কোনো Response না দিলে—

- Order Auto Cancel হবে।
- Customer Refund পাবে।
- Seller Performance-এ Record যুক্ত হবে।

---

# Rider Assignment

- শুধুমাত্র Admin Rider Assign করবে।
- Rider নিজে Order নিতে পারবে না।

---

# Rider Response

Rider

- Accept
- Reject
- অথবা No Response দিতে পারবে।

Reject অথবা No Response হলে Order আবার Admin-এর কাছে ফিরে যাবে।

---

# Same Shop Multiple Products

একই Shop-এর সব Product একটি Order-এ থাকবে।

Rider একবারেই সব Product Pickup এবং Delivery করবে।

---

# Delivery Failed

যদি Delivery ব্যর্থ হয়—

- Rider কারণ নির্বাচন করবে।
- প্রয়োজনে ছবি আপলোড করবে।
- Product Shop-এ ফেরত যাবে।

---

# Refund Rules

## Seller-এর কারণে Order বাতিল হলে

Customer পাবে—

- Product Price
- Delivery Charge

## Customer-এর কারণে Delivery ব্যর্থ হলে

Customer পাবে—

- শুধুমাত্র Product Price

Delivery Charge Refund হবে না।

---

# Payment

Customer Online Payment করবে।

Customer Wallet থাকবে না।

সব Payment প্রথমে HAT-BAZAR Escrow-এ জমা হবে।

---

# Payment Release

Delivery-এর ৩ দিন পরে Return না হলে Seller Wallet-এ Payment Release হবে।

---

# Order History

Customer এবং Seller দেখতে পারবে—

- Order ID
- Product
- Shop
- Amount
- Payment Status
- Delivery Status
- Date & Time

---

# Business Rules

- প্রতিটি Order-এর Unique Order ID থাকবে।
- সব Order Audit Log-এ সংরক্ষিত হবে।
- Rider Pickup শুরু হলে Cancel করা যাবে না।
- Seller কারণ ছাড়া Reject করতে পারবে না।
- Rider নিজে কোনো Order Claim করতে পারবে না।

---

# Future Scope

- Exchange System
- Scheduled Delivery
- Gift Order
- Partial Return

---

# Final Decision

- Multi Shop Checkout সমর্থিত।
- প্রতি Shop-এর জন্য আলাদা Order তৈরি হবে।
- Admin Rider Assign করবে।
- Customer Pickup শুরু হওয়ার আগে Cancel করতে পারবে।
- Seller Reject করতে পারবে।
- Rider Delivery Record সংরক্ষণ করবে।
- Delivery-এর ৩ দিন পরে Payment Release হবে।
- Refund Business Rule অনুযায়ী হবে।

---

Approved by:

HAT-BAZAR Blueprint Team

Version 1.0

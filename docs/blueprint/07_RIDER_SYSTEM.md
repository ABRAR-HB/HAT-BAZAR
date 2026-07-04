# HAT-BAZAR Rider System

**Document ID:** HB-RIDER-001
**Status:** Final
**Version:** 1.0
**Last Updated:** 2026-07-04

---

# Purpose

HAT-BAZAR Rider System এমনভাবে তৈরি করা হয়েছে যাতে প্রতিটি Pickup, Delivery এবং Return নিরাপদ, স্বচ্ছ ও প্রমাণযোগ্য হয়।

---

# Rider Recruitment

- HAT-BAZAR প্রয়োজন অনুযায়ী Rider নিয়োগের বিজ্ঞপ্তি প্রকাশ করবে।
- আগ্রহীরা সরাসরি HAT-BAZAR-এ আবেদন করবে।
- Rider Admin আবেদন যাচাই করবে।
- অনুমোদিত Rider-দের Rider App ব্যবহারের অনুমতি দেওয়া হবে।

---

# Rider Login

- Mobile Number
- OTP Verification

শুধুমাত্র অনুমোদিত Rider-রা Login করতে পারবে।

---

# Duty Status

Rider App-এ দুটি অবস্থা থাকবে:

- Online
- Offline

Business Rules:

- শুধুমাত্র Online Rider নতুন Delivery পাবে।
- Active Delivery থাকলে Offline হওয়া যাবে না।

---

# Rider Assignment

- Rider শুধুমাত্র Admin Assign করবে।
- Rider নিজে কোনো Order নিতে পারবে না।
- Reject বা No Response হলে Admin অন্য Rider Assign করবে।

---

# Product Pickup

Pickup করার সময় Rider:

- Product মিলিয়ে দেখবে।
- Product-এর ছবি তুলবে।
- Packaging-এর ছবি তুলবে।
- Seller-এর Digital Signature নেবে।
- Signature Receipt App-এ সংরক্ষণ করবে।
- তারপর Pickup Completed করবে।

---

# Product Delivery

Delivery করার সময় Rider:

- Customer-এর কাছে Product হস্তান্তর করবে।
- আগে থেকে তৈরি Digital Delivery Receipt খুলবে।
- Customer Signature নেবে।
- Signature Receipt-এর ছবি আপলোড করবে।
- তারপর Delivered করবে।

এরপর:

- Order Delivered হবে।
- ৩ দিনের Return Period শুরু হবে।

---

# Return Pickup

Return Approved হলে:

- Admin Rider Assign করবে।
- Rider Customer-এর কাছ থেকে Product সংগ্রহ করবে।
- Product-এর ছবি তুলবে।
- Customer-এর Signature নেবে।
- Product Shop-এ পৌঁছে দেবে।
- Seller-এর Signature নেবে।
- Return Complete করবে।

---

# Rider Wallet

Rider App-এ থাকবে:

- Available Balance
- Total Earnings
- Withdraw History

---

# Rider Income

Delivery Charge-এর:

- Rider → 70%
- HAT-BAZAR → 30%

Delivery সফল হওয়ার সাথে সাথেই Rider-এর আয় Wallet-এ যোগ হবে।

---

# Withdraw

Rider:

- মাসে একবার Withdraw করতে পারবে।

জরুরি প্রয়োজনে:

- Rider Admin Approval নিয়ে অতিরিক্ত Withdraw করতে পারবে।

Withdraw হবে:

- bKash
- Nagad
- Rocket

Automatic Process-এর মাধ্যমে।

---

# Business Rules

- Rider App শুধুমাত্র অনুমোদিত Rider-দের জন্য।
- Seller Signature ছাড়া Pickup Complete হবে না।
- Customer Signature ছাড়া Delivery Complete হবে না।
- Return-এর সময় Customer ও Seller—উভয়ের Signature বাধ্যতামূলক।
- সকল Signature, ছবি এবং Receipt স্থায়ীভাবে সংরক্ষণ করা হবে।
- সকল Rider Activity Audit Log-এ সংরক্ষণ করা হবে।

---

# Future Scope

- Rider Performance
- Rider Rating
- Rider Bonus
- Rider Suspension
- GPS Tracking

---

# Final Decision

- Rider Admin দ্বারা নিয়োগ হবে।
- Rider App শুধুমাত্র অনুমোদিত Rider ব্যবহার করবে।
- Pickup-এ Seller Signature বাধ্যতামূলক।
- Delivery-তে Customer Signature বাধ্যতামূলক।
- Return-এ উভয় পক্ষের Signature বাধ্যতামূলক।
- Rider Delivery Charge-এর 70% পাবে।
- Rider মাসে একবার Withdraw করতে পারবে।
- জরুরি Withdraw-এর জন্য Admin Approval লাগবে।

---

Approved by:

HAT-BAZAR Blueprint Team

Version 1.0

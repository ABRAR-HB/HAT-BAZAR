# HAT-BAZAR Payment System

**Document ID:** HB-PAYMENT-001
**Status:** Final
**Version:** 1.0
**Last Updated:** 2026-07-04

---

# Purpose

HAT-BAZAR-এর Payment System এমনভাবে ডিজাইন করা হয়েছে যাতে Customer, Seller এবং HAT-BAZAR-এর মধ্যে সকল আর্থিক লেনদেন নিরাপদ, স্বয়ংক্রিয় এবং স্বচ্ছভাবে পরিচালিত হয়।

---

# Customer Payment

## Customer Wallet

- Customer-এর কোনো Wallet থাকবে না।

## Payment Method

Customer প্রতিবার Order করার সময় অনলাইনে Payment করবে।

সমর্থিত মাধ্যম:

- bKash
- Nagad
- Rocket
- Card
- ভবিষ্যতে অন্যান্য Online Payment Gateway

---

# Escrow System

Customer-এর সম্পূর্ণ Payment প্রথমে HAT-BAZAR Escrow Account-এ জমা হবে।

Seller সঙ্গে সঙ্গে টাকা পাবে না।

Product Delivered হওয়ার পরে ৩ দিনের Return Period শুরু হবে।

যদি Return না আসে, তাহলে Product-এর মূল্য Seller Wallet-এ Release করা হবে।

---

# Seller Wallet

লোকেশন:

My Shop → My Wallet

Wallet-এ থাকবে:

- Available Balance
- Pending Balance
- Pending Release
- Transaction History

---

# Transaction History

প্রতিটি Transaction-এ থাকবে:

- Order ID
- Product Name
- Shop Name
- Amount
- Date & Time
- Status

Seller Product Name-এ ক্লিক করলে Product Details দেখতে পারবে।

Order ID-তে ক্লিক করলে Order Details দেখতে পারবে।

---

# Withdraw System

Seller যখন ইচ্ছা তখন Withdraw করতে পারবে।

শুধুমাত্র Available Balance Withdraw করা যাবে।

Pending Balance অথবা Pending Release Withdraw করা যাবে না।

সমর্থিত মাধ্যম:

- bKash
- Nagad
- Rocket

ভবিষ্যতে:

- Bank Account

Withdraw সম্পূর্ণ Automatic হবে।

---

# Withdraw Status

Withdraw Status:

- Processing
- Success
- Failed

---

# Refund

Return Approved হলে:

- Product Seller-এর কাছে ফেরত যাবে।
- Customer Product-এর মূল্য ফেরত পাবে।
- Refund একই Payment Method-এ পাঠানো হবে।
- Delivery Charge Refund হবে না।

---

# Support System

যদি Withdraw বা Payment-এ সমস্যা হয়,

Seller Support Ticket খুলতে পারবে।

Ticket-এ থাকবে:

- Ticket ID
- Transaction ID
- Status
- Chat

---

# Admin Support

Finance Admin প্রয়োজনে Seller-এর সাথে Ticket-এর ভিতরে Chat করতে পারবেন।

সমস্ত Chat এবং Transaction Audit Log-এ সংরক্ষণ করা হবে।

---

# Future Scope

- Bank Withdraw
- International Payment Gateway
- Auto Rent Deduction
- Financial Report

---

# Final Decision

- Customer Wallet থাকবে না।
- Seller Wallet থাকবে।
- Escrow ব্যবহার করা হবে।
- ৩ দিন পরে Payment Release হবে।
- Withdraw সম্পূর্ণ Automatic হবে।
- Seller যেকোনো সময় Available Balance Withdraw করতে পারবে।
- Refund একই Payment Method-এ যাবে।
- Delivery Charge Refund হবে না।
- Payment সমস্যার জন্য Finance Admin Support থাকবে।

---

Approved by:
HAT-BAZAR Blueprint
Version 1.0

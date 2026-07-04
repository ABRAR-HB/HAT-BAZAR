# HAT-BAZAR Notification System

**Document ID:** HB-NOTIFICATION-001
**Status:** Final
**Version:** 1.0
**Last Updated:** 2026-07-04

---

# Purpose

HAT-BAZAR Notification System এমনভাবে তৈরি করা হয়েছে যাতে Customer, Seller এবং Rider সকল গুরুত্বপূর্ণ ঘটনার তাৎক্ষণিক তথ্য পায়।

---

# Notification Types

## Customer

- Order Confirmed
- Order Packed
- Rider Assigned
- Rider Picked Up Product
- Out for Delivery
- Order Delivered
- Return Approved
- Return Rejected
- Refund Completed
- New Chat Message
- Coupon Received
- Offer & Campaign
- Security Alert

---

## Seller

- New Order
- New Chat Message
- Rider Assigned
- Rider Arrived for Pickup
- Product Picked Up
- Product Returned
- Wallet Balance Added
- Withdraw Completed
- Admin Notice

---

## Rider

- New Delivery Assigned
- Pickup Reminder
- Delivery Reminder
- Return Pickup Assigned
- Wallet Balance Added
- Withdraw Completed
- Admin Notice

---

# Push Notifications

- সকল গুরুত্বপূর্ণ Notification Push Notification হিসেবে পাঠানো হবে।
- Notification-এ চাপ দিলে সংশ্লিষ্ট Page খুলবে।

---

# Notification Center

Notification Center-এ থাকবে:

- Unread Notifications
- Read Notifications

User পারবে:

- একটি Notification Read করতে।
- সব Notification Read করতে।
- একটি Notification Delete করতে।
- সব Notification Clear করতে।

---

# Notification Settings

## Customer

- Order Updates
- Chat Notifications
- Offer & Campaign
- Coupon

## Seller

- New Order
- Chat
- Wallet
- Withdraw

## Rider

- Delivery
- Wallet
- Withdraw

---

# Mandatory Notifications

নিচের Notification কখনো বন্ধ করা যাবে না:

- Security Alert
- Admin Emergency Notice

---

# Smart Notifications

একই ধরনের Notification Group করা হবে।

উদাহরণ:

- You have 4 new messages.
- You have 10 new orders.

Security Alert এবং Withdraw Notification সবসময় আলাদা দেখানো হবে।

---

# Scheduled Notifications

System স্বয়ংক্রিয়ভাবে Reminder পাঠাবে।

যেমন:

- Cart Reminder
- Seller Order Reminder
- Rider Pickup Reminder
- Rider Delivery Reminder
- Monthly Withdraw Reminder

---

# Business Rules

- Notification Newest First দেখানো হবে।
- গুরুত্বপূর্ণ Notification নির্দিষ্ট সময় পর্যন্ত সংরক্ষিত থাকবে।
- App বন্ধ থাকলেও Push Notification যাবে।
- সাধারণ Reminder User চালু/বন্ধ করতে পারবে।
- Security Reminder বন্ধ করা যাবে না।

---

# Future Scope

- Email Notification
- SMS Notification
- WhatsApp Notification
- Browser Notification
- AI Smart Notification Priority

---

# Final Decision

- Push Notification থাকবে।
- Notification Center থাকবে।
- Notification Settings থাকবে।
- Smart Group Notification থাকবে।
- Scheduled Reminder থাকবে।
- Security Alert সবসময় বাধ্যতামূলক থাকবে।

---

Approved by:

HAT-BAZAR Blueprint Team

Version 1.0

# CHAT ROOMS TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The chat_rooms table manages conversations between users within the HAT-BAZAR platform.

A chat room represents a dedicated communication channel where participants can exchange messages related to products, orders, customer support, or administrative activities.

Each chat room contains multiple chat messages stored separately in the chat_messages table.

---

# Table Name

chat_rooms

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Each chat room is created when two or more participants begin a conversation.

The room remains available even after all messages are deleted for auditing and historical purposes.

Chat rooms support:

Buyer ↔ Seller

Buyer ↔ Admin

Seller ↔ Admin

Buyer ↔ Rider (Future)

Group Support (Future)

---

# Ownership

Each room belongs to multiple participants.

Participants are managed through a separate participant mapping table in future versions.

Each room can contain unlimited messages.

---

# Main Columns

id

room_uuid

room_type

title

created_by

last_message_id

last_message_at

status

created_at

updated_at

deleted_at

---

# Room Types

private

support

order

admin

system

group (Future)

---

# Business Rules

Every room must have at least two participants.

Room UUID must be globally unique.

Soft delete must be supported.

Messages remain linked to their original room.

Deleting a room never deletes message history.

---

# Usage

The chat room system supports:

Customer support

Product inquiries

Order communication

Seller assistance

Admin communication

Future real-time messaging

---

# Supported Status

active

archived

locked

deleted

---

# User Experience

Users can:

Create conversations

Continue previous chats

Search conversations

Archive conversations

Receive real-time notifications

Resume conversations after reconnecting
# Column Definitions

---

## id

Type

BIGSERIAL

Primary Key

Not Null

---

## room_uuid

Type

UUID

Not Null

Unique

System-generated globally unique identifier for the chat room.

---

## room_type

Type

VARCHAR(30)

Not Null

Allowed Values

private

support

order

admin

system

group

---

## title

Type

VARCHAR(255)

Nullable

Optional display name for support or group conversations.

Private chats normally do not require a title.

---

## created_by

Type

BIGINT

Foreign Key

References

users(id)

Not Null

User who created the room.

---

## last_message_id

Type

BIGINT

Foreign Key

References

chat_messages(id)

Nullable

Stores the latest message for quick conversation listing.

---

## last_message_at

Type

TIMESTAMP

Nullable

Updated whenever a new message is sent.

---

## status

Type

VARCHAR(20)

Default

active

Allowed Values

active

archived

locked

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

chat_rooms.created_by

→ users.id

chat_rooms.last_message_id

→ chat_messages.id

---

# Constraints

Room UUID must be unique.

Every room must have one creator.

Room status must be a valid value.

Locked rooms cannot accept new messages.

Deleted rooms remain available for auditing.

---

# Indexes

PRIMARY KEY (id)

UNIQUE INDEX (room_uuid)

INDEX (created_by)

INDEX (room_type)

INDEX (status)

INDEX (last_message_at)

Composite Index

(room_type, status)

(created_by, status)

---

# Room Lifecycle

Room Created

↓

Participants Added

↓

Messages Sent

↓

Room Archived (Optional)

↓

Room Locked (Optional)

↓

Soft Deleted (Optional)

Message history is preserved throughout the lifecycle.

---

# Security & Privacy

Only room participants may access messages.

Authentication is required for all chat APIs.

Authorization must be verified for every request.

Room identifiers should never be predictable.

Soft deleted rooms remain hidden from users but available for administrative audit.

Chat metadata must never expose private user information to unauthorized users.
# Order Integration

Order-related chat rooms can be automatically created after an order is placed.

The room may include:

Buyer

Seller

Support Team (optional)

Future Rider

The room helps resolve:

Delivery questions

Order updates

Product issues

Return requests

Refund discussions

Each order references its associated chat room.

---

# Notification Integration

The notification service integrates with chat rooms.

Users receive notifications for:

New messages

Unread messages

Support replies

Order updates

Mentions (Future)

Notifications may be delivered through:

In-app notifications

Push notifications

Email (optional)

SMS (optional)

---

# Real-Time Messaging Integration

The chat room serves as the communication channel for real-time messaging.

Future technologies may include:

WebSocket

Supabase Realtime

Firebase Cloud Messaging

Server-Sent Events (SSE)

The room metadata stores only conversation information.

Actual message content is stored in the chat_messages table.

---

# Example Records

## Example 1

Room UUID

550e8400-e29b-41d4-a716-446655440000

Room Type

private

Created By

25

Status

active

Last Message At

2026-07-09 10:45:12

---

## Example 2

Room UUID

91c74120-1ab7-4f2d-85aa-2f4c7b8d8f10

Room Type

order

Created By

17

Status

archived

Last Message At

2026-07-08 18:20:55

---

# Future Expansion

Future versions may support:

Group conversations

Voice messages

Video calling

File sharing

Image sharing

Typing indicators

Message reactions

Pinned conversations

Chat search

AI customer support assistant

Automatic translation

End-to-end encryption

---

# Best Practices

Always use UUID for room identification.

Never permanently delete chat history.

Archive inactive rooms instead of deleting them.

Validate participant permissions before every operation.

Keep room metadata separate from message content.

Optimize room listing using last_message_at indexes.

Maintain audit logs for administrative actions.

---

# Conclusion

The chat_rooms table provides a scalable and secure foundation for the HAT-BAZAR communication system.

It supports:

Private conversations

Order-related communication

Customer support

Administrative messaging

Real-time messaging integration

Future collaboration features

This design ensures secure, organized, and extensible communication while preserving conversation history and supporting future platform growth.

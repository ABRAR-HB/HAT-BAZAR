# CHAT MESSAGES TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The chat_messages table stores every message exchanged inside chat rooms.

Each record represents one individual message sent by a participant.

The system supports text messages, images, files, system notifications, and future multimedia communication.

This table is designed for scalability, security, real-time messaging, and long-term message history.

---

# Table Name

chat_messages

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Every message belongs to exactly one chat room.

Each message has one sender.

Messages remain permanently linked to their room even if users leave the conversation.

Messages support editing, deletion, delivery tracking, and read receipts.

---

# Ownership

Each message belongs to:

One chat room

One sender

Optional attachment

Optional reply message

---

# Main Columns

id

message_uuid

room_id

sender_id

reply_to_message_id

message_type

message_text

attachment_url

attachment_type

delivery_status

read_status

edited

edited_at

created_at

updated_at

deleted_at

---

# Message Types

text

image

video

audio

document

location

system

order_update

payment_update

---

# Business Rules

Every message belongs to one room.

Every message has one sender.

Empty messages are not allowed.

Messages can include attachments.

Soft delete must be supported.

Edited messages preserve edit history.

---

# Usage

The messaging system supports:

Private chat

Order communication

Customer support

Administrative announcements

Media sharing

Future voice messaging

---

# Delivery Status

sending

sent

delivered

read

failed

---

# User Experience

Users can:

Send messages

Receive messages

Edit messages

Delete messages

Reply to messages

Forward messages (Future)

Send images and files

Receive delivery confirmations

See read receipts
# Column Definitions

---

## id

Type

BIGSERIAL

Primary Key

Not Null

---

## message_uuid

Type

UUID

Not Null

Unique

System-generated globally unique identifier.

---

## room_id

Type

BIGINT

Foreign Key

References

chat_rooms(id)

Not Null

Indexed

Chat room containing this message.

---

## sender_id

Type

BIGINT

Foreign Key

References

users(id)

Not Null

Indexed

User who sent the message.

---

## reply_to_message_id

Type

BIGINT

Foreign Key

References

chat_messages(id)

Nullable

Supports threaded replies.

---

## message_type

Type

VARCHAR(30)

Not Null

Allowed Values

text

image

video

audio

document

location

system

order_update

payment_update

---

## message_text

Type

TEXT

Nullable

Required for text messages.

Optional for media messages.

---

## attachment_url

Type

TEXT

Nullable

Storage location of uploaded media or document.

---

## attachment_type

Type

VARCHAR(50)

Nullable

Example

image/jpeg

image/png

video/mp4

application/pdf

audio/mpeg

---

## delivery_status

Type

VARCHAR(20)

Default

sending

Allowed Values

sending

sent

delivered

read

failed

---

## read_status

Type

BOOLEAN

Default

FALSE

---

## edited

Type

BOOLEAN

Default

FALSE

---

## edited_at

Type

TIMESTAMP

Nullable

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

chat_messages.room_id

→ chat_rooms.id

chat_messages.sender_id

→ users.id

chat_messages.reply_to_message_id

→ chat_messages.id

---

# Constraints

Every message must belong to one room.

Every message must have one sender.

At least one of message_text or attachment_url must be present.

Only valid message types are allowed.

Reply messages must reference a message from the same room.

Deleted messages remain available for auditing.

---

# Indexes

PRIMARY KEY (id)

UNIQUE INDEX (message_uuid)

INDEX (room_id)

INDEX (sender_id)

INDEX (delivery_status)

INDEX (created_at)

Composite Index

(room_id, created_at)

(sender_id, created_at)

---

# Reply System

Users can reply to any previous message.

Replies maintain a reference to the original message.

Deleting the original message does not remove replies.

The application may display reply previews.

---

# Attachment System

Messages may include:

Images

Videos

Audio

Documents

Location sharing

Future stickers

Future GIFs

Large files should be stored in object storage while only the URL is saved in the database.

---

# Security & Privacy

Only authorized participants may access messages.

Authentication is required for all messaging APIs.

Attachment URLs should be protected from unauthorized access.

Server-side validation is required for every message.

Soft deleted messages remain hidden from users but available for audit purposes.
# Read Receipt Integration

The messaging system supports read receipts for improved communication.

When a participant opens a conversation:

The message status changes from:

sent

↓

delivered

↓

read

Read timestamps may be stored in a separate message_reads table for group conversations in future versions.

---

# Notification Integration

The notification service integrates with chat messages.

Notifications are generated for:

New messages

Replies

Support responses

Order updates

Payment updates

Future mentions

Delivery channels include:

In-app notifications

Push notifications

Email (optional)

SMS (optional)

Notifications should respect user preferences.

---

# Edit & Delete History

Messages may be edited after sending.

When edited:

edited = TRUE

edited_at is updated

The original message may be stored in an audit log for administrative purposes.

Soft deletion hides the message from users while preserving it for compliance and investigations.

---

# Example Records

## Example 1

Message UUID

550e8400-e29b-41d4-a716-446655440111

Room ID

15

Sender ID

25

Message Type

text

Message

Is this product available?

Delivery Status

read

Edited

No

---

## Example 2

Message UUID

91c74120-1ab7-4f2d-85aa-2f4c7b8d8f99

Room ID

15

Sender ID

7

Message Type

image

Attachment

products/sample-image.jpg

Delivery Status

delivered

Edited

No

---

# Future Expansion

Future versions may support:

Voice messages

Video messages

Message reactions

Emoji support

Typing indicators

Message pinning

Message search

Message translation

Scheduled messages

Disappearing messages

End-to-end encryption

AI message summarization

---

# Best Practices

Always use UUID for external message references.

Store large media files outside the database.

Never permanently delete production messages.

Validate sender permissions before accepting messages.

Protect attachment URLs using secure access controls.

Index messages by room and creation time for fast loading.

Keep audit logs for edits and moderation actions.

---

# Conclusion

The chat_messages table provides a secure, scalable, and production-ready messaging foundation for HAT-BAZAR.

It supports:

Text and media messaging

Replies

Delivery tracking

Read receipts

Message editing

Soft deletion

Real-time communication

Future AI-powered messaging features

This design ensures reliable communication while maintaining security, auditability, and long-term scalability across the marketplace platform.

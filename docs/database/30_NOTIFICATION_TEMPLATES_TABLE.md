# NOTIFICATION TEMPLATES TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The notification_templates table stores reusable notification templates used throughout the HAT-BAZAR platform.

Templates standardize notification content across multiple delivery channels while supporting personalization, localization, version control, and future communication channels.

---

# Table Name

notification_templates

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Each template defines the structure and content of a notification.

Templates may be reused by multiple notifications and different system modules.

Dynamic variables are replaced with actual values during notification generation.

---

# Ownership

Each template belongs to:

The HAT-BAZAR platform

Optional administrator who created or updated it

---

# Main Columns

id

template_uuid

template_code

template_name

notification_type

supported_channels

language_code

subject

message_body

variables

is_active

version

created_by

updated_by

created_at

updated_at

---

# Notification Types

order

payment

refund

delivery

promotion

system

security

chat

wallet

general

---

# Supported Channels

in_app

push

email

sms

webhook

future_whatsapp

---

# Business Rules

Every template must have a unique template code.

Templates may support multiple delivery channels.

Inactive templates cannot be used for new notifications.

Template variables should follow a consistent placeholder format.

---

# Usage

The template system supports:

Reusable notification content

Multi-language messaging

Personalized notifications

Centralized template management

Future A/B testing

Version management

---

# Variable Format

Dynamic variables use double curly braces.

Examples

{{customer_name}}

{{order_number}}

{{shop_name}}

{{tracking_number}}

{{refund_amount}}


# Column Definitions

---

## id

Type

BIGSERIAL

Primary Key

Not Null

---

## template_uuid

Type

UUID

Not Null

Unique

System-generated globally unique template identifier.

---

## template_code

Type

VARCHAR(100)

Not Null

Unique

Human-readable unique template code.

Examples

ORDER_CONFIRMED

PAYMENT_SUCCESS

REFUND_COMPLETED

DELIVERY_ASSIGNED

---

## template_name

Type

VARCHAR(255)

Not Null

Descriptive template name.

---

## notification_type

Type

VARCHAR(30)

Not Null

Allowed Values

order

payment

refund

delivery

promotion

system

security

chat

wallet

general

---

## supported_channels

Type

JSONB

Not Null

Stores one or more supported delivery channels.

Example

["push","email","sms"]

---

## language_code

Type

VARCHAR(10)

Default

en

Examples

en

bn

ar

hi

---

## subject

Type

VARCHAR(255)

Nullable

Email or push notification subject.

---

## message_body

Type

TEXT

Not Null

Notification template body containing variables.

---

## variables

Type

JSONB

Nullable

List of supported template variables.

Example

[
  "customer_name",
  "order_number",
  "tracking_number"
]

---

## is_active

Type

BOOLEAN

Default

TRUE

Inactive templates cannot be selected for new notifications.

---

## version

Type

INTEGER

Default

1

Template version number.

---

## created_by

Type

BIGINT

Foreign Key

References

users(id)

Nullable

Administrator who created the template.

---

## updated_by

Type

BIGINT

Foreign Key

References

users(id)

Nullable

Administrator who last updated the template.

---

## created_at

TIMESTAMP

Default CURRENT_TIMESTAMP

---

## updated_at

TIMESTAMP

Auto Updated

---

# Foreign Keys

notification_templates.created_by

→ users.id

notification_templates.updated_by

→ users.id

---

# Constraints

Template UUID must be unique.

Template code must be unique.

Message body cannot be empty.

Version must be greater than zero.

Only active templates may be used for notification generation.

Supported channels must contain valid channel names.

---

# Indexes

PRIMARY KEY (id)

UNIQUE INDEX (template_uuid)

UNIQUE INDEX (template_code)

INDEX (notification_type)

INDEX (language_code)

INDEX (is_active)

INDEX (created_at)

Composite Index

(notification_type, language_code)

(notification_type, is_active)

(template_code, version)

---

# Template Rendering Rules

The notification engine replaces variables with runtime values.

Missing required variables should prevent notification generation.

Variable names are case-sensitive.

Rendered notifications should preserve formatting across all supported channels.

---

# Version Management

Each template starts with version 1.

Major template updates should increment the version number.

Older versions may be retained for audit and rollback purposes.

Only one active version of the same template code should normally be used in production.

---

# Security & Administration Rules

Only authorized administrators may create or modify templates.

All template changes should be recorded in the audit log.

Template previews should be available before publishing.

Templates containing sensitive information must be reviewed before activation.

# Example Templates

## Example 1

Template Code

ORDER_CONFIRMED

Notification Type

order

Language

en

Subject

Your Order Has Been Confirmed

Message

Hello {{customer_name}}, your order #{{order_number}} has been successfully confirmed.

---

## Example 2

Template Code

DELIVERY_ASSIGNED

Notification Type

delivery

Language

en

Subject

Delivery Partner Assigned

Message

Your order #{{order_number}} has been assigned to a delivery partner.

Tracking Number: {{tracking_number}}

---

## Example 3

Template Code

REFUND_COMPLETED

Notification Type

refund

Language

en

Subject

Refund Completed

Message

Your refund of {{refund_amount}} has been successfully processed.

---

# Multi-language Support

Templates may exist in multiple languages.

Example:

English

Bengali

Arabic

Hindi

Future language support can be added without changing application logic.

The notification engine should automatically select the user's preferred language whenever available.

---

# Analytics

Notification template usage supports:

Most frequently used templates

Delivery success rate by template

Open rate by template

Channel performance comparison

Language usage statistics

Template version usage

Template effectiveness reporting

These insights help improve customer communication.

---

# Future Expansion

Future versions may support:

Visual email templates

Rich HTML templates

Markdown support

A/B testing

AI-generated notification content

Automatic translation

Template approval workflow

Scheduled template activation

Multi-brand template management

---

# Best Practices

Use meaningful template codes.

Keep templates short and easy to understand.

Avoid hardcoded values.

Use variables for dynamic content.

Maintain separate templates for each language.

Review templates before activation.

Archive obsolete template versions instead of deleting them.

---

# Conclusion

The notification_templates table provides a centralized, reusable, and scalable notification template management system for HAT-BAZAR.

It supports:

Reusable templates

Multiple notification channels

Dynamic variables

Multi-language messaging

Template versioning

Centralized administration

Future AI-assisted messaging

This design ensures consistent communication, easier maintenance, improved user experience, and long-term scalability across the HAT-BAZAR platform.

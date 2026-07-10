# PLATFORM SETTINGS TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The platform_settings table stores global configuration values used across the HAT-BAZAR platform.

It centralizes system-wide settings such as platform information, commission rates, payment options, wallet configuration, notification preferences, maintenance mode, and feature flags.

The table allows administrators to update operational settings without modifying application code.

---

# Table Name

platform_settings

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Each record represents one configurable platform setting.

Every setting is identified by a unique key and stores its corresponding value.

Settings are loaded by different system modules during runtime.

---

# Ownership

Each setting belongs to:

The HAT-BAZAR platform

Optional administrator who last updated it

---

# Main Columns

id

setting_key

setting_value

setting_type

category

description

is_public

is_editable

updated_by

created_at

updated_at

---

# Setting Types

string

integer

decimal

boolean

json

text

---

# Business Rules

Each setting key must be unique.

Settings may be public or internal.

Only authorized administrators may edit settings.

Critical settings should require audit logging.

---

# Usage

The settings system supports:

Platform configuration

Commission configuration

Wallet configuration

Payment gateway configuration

Notification configuration

Maintenance mode

Feature flags

Future system modules

---

# Categories

general

finance

payment

wallet

delivery

notification

security

feature_flag

system


# Column Definitions

---

## id

Type

BIGSERIAL

Primary Key

Not Null

---

## setting_key

Type

VARCHAR(150)

Not Null

Unique

System-wide unique configuration key.

Examples

platform_name

default_commission_rate

maintenance_mode

wallet_enabled

---

## setting_value

Type

TEXT

Not Null

Stores the configuration value.

The actual format depends on the setting type.

---

## setting_type

Type

VARCHAR(20)

Not Null

Allowed Values

string

integer

decimal

boolean

json

text

---

## category

Type

VARCHAR(30)

Not Null

Allowed Values

general

finance

payment

wallet

delivery

notification

security

feature_flag

system

---

## description

Type

TEXT

Nullable

Explanation of the purpose of the setting.

---

## is_public

Type

BOOLEAN

Default

FALSE

Indicates whether the setting can be exposed to client applications.

---

## is_editable

Type

BOOLEAN

Default

TRUE

Indicates whether administrators can modify the setting.

---

## updated_by

Type

BIGINT

Foreign Key

References

users(id)

Nullable

Administrator who last updated the setting.

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

platform_settings.updated_by

→ users.id

---

# Constraints

Setting key must be unique.

Setting value cannot be NULL.

Only supported setting types are allowed.

System-critical settings marked as non-editable cannot be modified through the admin panel.

Public settings must not contain confidential information.

---

# Indexes

PRIMARY KEY (id)

UNIQUE INDEX (setting_key)

INDEX (category)

INDEX (setting_type)

INDEX (is_public)

Composite Index

(category, setting_key)

(category, is_public)

---

# Configuration Rules

Every module reads configuration using the setting_key.

Configuration values should be validated before saving.

Changes to critical settings should take effect only after successful validation.

JSON settings must contain valid JSON.

Invalid configuration changes must be rejected.

---

# Security & Audit Requirements

Only authorized administrators may create or modify settings.

Every configuration change should be recorded in the audit log.

Sensitive configuration values should be encrypted when appropriate.

Configuration backups should be included in disaster recovery plans.

Access to internal settings must be restricted according to administrator roles.


# Example Settings

## Example 1

Setting Key

platform_name

Value

HAT-BAZAR

Type

string

Category

general

Public

Yes

---

## Example 2

Setting Key

default_commission_rate

Value

10.00

Type

decimal

Category

finance

Public

No

---

## Example 3

Setting Key

maintenance_mode

Value

false

Type

boolean

Category

system

Public

Yes

---

## Example 4

Setting Key

wallet_enabled

Value

true

Type

boolean

Category

wallet

Public

Yes

---

## Example 5

Setting Key

supported_payment_methods

Value

["bkash","nagad","rocket","card","wallet"]

Type

json

Category

payment

Public

Yes

---

# Feature Flag Integration

The platform supports feature flags to enable or disable functionality without deploying new code.

Examples include:

Enable wallet

Enable live chat

Enable express delivery

Enable AI recommendations

Enable referral program

Enable loyalty points

Enable seller analytics

Each feature flag should be independently configurable.

---

# Environment Configuration

Different deployment environments may override selected settings.

Examples:

Development

Testing

Staging

Production

Environment-specific values should be managed securely and should not overwrite permanent business configuration unintentionally.

---

# Future Expansion

Future versions may support:

Environment-specific overrides

Configuration versioning

Rollback support

Encrypted secret management

Distributed configuration synchronization

Dynamic cache invalidation

Real-time configuration updates

Multi-region configuration

AI-assisted configuration recommendations

---

# Best Practices

Use descriptive and consistent setting keys.

Validate all configuration values before saving.

Keep sensitive configuration values encrypted.

Avoid storing secrets in plain text.

Maintain complete audit logs for every configuration change.

Back up configuration regularly.

Test configuration changes in staging before production deployment.

---

# Conclusion

The platform_settings table provides a centralized, secure, and scalable configuration management foundation for HAT-BAZAR.

It supports:

Global platform configuration

Financial settings

Payment and wallet configuration

Delivery settings

Notification preferences

Feature flags

System administration

Future environment-aware configuration

This design enables flexible platform management while ensuring security, consistency, maintainability, and long-term scalability across the entire marketplace ecosystem.

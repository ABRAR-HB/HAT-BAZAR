# AUDIT LOGS TABLE

Version: 1.0

Status: Official Database Design

---

# Purpose

The audit_logs table records important system activities performed by users, sellers, riders, administrators, and automated services across the HAT-BAZAR platform.

It provides a permanent audit trail for security monitoring, operational transparency, compliance, fraud investigation, and troubleshooting.

Audit logs should remain immutable and searchable for long-term analysis.

---

# Table Name

audit_logs

---

# Primary Key

id

BIGSERIAL

Auto Increment

---

# Description

Each audit log represents one recorded system event.

Events may originate from user actions, administrator operations, API requests, authentication events, or automated background jobs.

Logs are append-only and should never be modified after creation.

---

# Ownership

Each audit log belongs to:

Optional user

Optional administrator

Optional API client

Optional system service

---

# Main Columns

id

log_uuid

user_id

action

resource_type

resource_id

old_value

new_value

ip_address

user_agent

device_type

request_id

status

created_at

---

# Action Types

create

update

delete

login

logout

password_change

payment

refund

order_status_change

system

api_request

permission_change

---

# Business Rules

Every audit log records exactly one event.

Audit logs are immutable.

Sensitive operations must always generate audit logs.

Soft delete is not recommended.

---

# Usage

The audit log system supports:

Security monitoring

Compliance reporting

Fraud investigation

Administrative auditing

API request tracking

Operational troubleshooting

Future SIEM integration

---

# Log Status

success

failed

warning

blocked


# Column Definitions

---

## id

Type

BIGSERIAL

Primary Key

Not Null

---

## log_uuid

Type

UUID

Not Null

Unique

System-generated globally unique audit log identifier.

---

## user_id

Type

BIGINT

Foreign Key

References

users(id)

Nullable

User responsible for the action.

For automated system events, this field may be NULL.

---

## action

Type

VARCHAR(50)

Not Null

Allowed Values

create

update

delete

login

logout

password_change

payment

refund

order_status_change

system

api_request

permission_change

---

## resource_type

Type

VARCHAR(100)

Not Null

Examples

users

orders

products

payments

refunds

shops

inventory

settings

---

## resource_id

Type

BIGINT

Nullable

Primary key of the affected resource.

---

## old_value

Type

JSONB

Nullable

Stores the previous values before modification.

---

## new_value

Type

JSONB

Nullable

Stores the updated values after modification.

---

## ip_address

Type

INET

Nullable

IP address from which the action originated.

---

## user_agent

Type

TEXT

Nullable

Browser, application, or client information.

---

## device_type

Type

VARCHAR(30)

Nullable

Examples

mobile

desktop

tablet

api

system

---

## request_id

Type

UUID

Nullable

Unique identifier for tracing a request across services.

---

## status

Type

VARCHAR(20)

Default

success

Allowed Values

success

failed

warning

blocked

---

## created_at

TIMESTAMP

Default CURRENT_TIMESTAMP

Not Null

---

# Foreign Keys

audit_logs.user_id

→ users.id

---

# Constraints

Log UUID must be unique.

Action type must be valid.

Audit logs cannot be modified after creation.

Audit logs cannot be deleted under normal operations.

Old and new values should contain valid JSON when present.

---

# Indexes

PRIMARY KEY (id)

UNIQUE INDEX (log_uuid)

INDEX (user_id)

INDEX (action)

INDEX (resource_type)

INDEX (resource_id)

INDEX (status)

INDEX (created_at)

Composite Index

(user_id, created_at)

(resource_type, resource_id)

(action, created_at)

---

# Audit Recording Rules

The platform should automatically generate audit logs for:

User authentication

Administrative actions

Permission changes

Financial transactions

Inventory modifications

Platform configuration changes

Sensitive data updates

API requests requiring traceability

---

# Security & Compliance Requirements

Audit logs must be immutable.

Access should be restricted to authorized administrators.

Sensitive information should be masked where appropriate.

Audit records should be retained according to legal and business requirements.

Audit logs should be included in backup and disaster recovery procedures.


# Example Records

## Example 1

Log UUID

550e8400-e29b-41d4-a716-446655440999

User ID

25

Action

login

Resource Type

users

Resource ID

25

IP Address

103.125.44.12

Device Type

mobile

Status

success

Created At

2026-07-10 09:15:20

---

## Example 2

Log UUID

91c74120-1ab7-4f2d-85aa-2f4c7b8d8fcc

User ID

3

Action

update

Resource Type

platform_settings

Resource ID

7

Status

success

Created At

2026-07-10 10:32:18

---

## Example 3

Log UUID

a12f8400-e29b-41d4-a716-446655441111

User ID

NULL

Action

system

Resource Type

inventory

Status

warning

Created At

2026-07-10 11:05:40

---

# Compliance & Investigation

Audit logs support:

Security investigations

Fraud detection

Administrative accountability

Financial audits

Regulatory compliance

Incident response

Forensic analysis

Historical activity review

Every significant system action should be traceable through audit records.

---

# Analytics

Audit logs provide valuable operational insights, including:

Successful and failed login trends

Administrative activity reports

API usage statistics

Most frequently modified resources

Permission change history

Security incident reports

Failed operation analysis

User activity timelines

These reports help improve platform security and operational efficiency.

---

# Future Expansion

Future versions may support:

SIEM integration

Real-time security alerts

Anomaly detection using AI

Centralized log aggregation

Cross-service request tracing

Risk scoring

Tamper detection

Compliance dashboards

Long-term archive storage

---

# Best Practices

Generate audit logs automatically for sensitive operations.

Never modify or delete audit records.

Mask sensitive personal information where required.

Retain logs according to legal and business policies.

Secure audit logs against unauthorized access.

Monitor unusual activity patterns regularly.

Archive old logs without affecting production performance.

---

# Conclusion

The audit_logs table provides a secure, immutable, and production-ready audit trail for HAT-BAZAR.

It supports:

User activity tracking

Administrative auditing

Security monitoring

Financial traceability

Compliance reporting

Fraud investigation

Future enterprise security integrations

This design ensures accountability, transparency, and long-term operational security while supporting enterprise-grade governance across the entire HAT-BAZAR platform.

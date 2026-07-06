# HAT-BAZAR Blueprint

# User Authentication System

Version: 1.0

Status: Final Blueprint

---

# Purpose

The User Authentication System provides secure registration, login, identity verification and account protection for all HAT-BAZAR users.

The system supports different authentication methods while maintaining marketplace security and user privacy.

---

# Scope

This Blueprint defines:

- Customer Authentication
- Seller Authentication
- Rider Authentication
- Registration Rules
- Login Rules
- OTP Verification
- KYC Verification
- Account Status
- Business Rules

---

# Core Principles

The User Authentication System follows these principles:

- Security First
- Identity Verification
- One Verified Identity
- Role-Based Access
- Privacy Protection

Every authenticated user must follow the official HAT-BAZAR verification process.

---

# User Roles

The marketplace supports three primary user roles:

- Customer
- Seller
- Rider

Every role has its own authentication and verification requirements.

---

# Customer Registration

Customers may register using either:

- Google Sign-In

or

- Mobile Number
- OTP Verification
- Full Name

National ID (NID) is not required for Customer registration.

---

# Customer Login

Customers may log in using:

- Google Account

or

- Mobile Number
- OTP
- Password

---

# Seller Registration

Seller registration requires:

- Full Name
- Mobile Number
- OTP Verification
- Date of Birth
- National ID (NID) Number
- NID Front Photo
- NID Back Photo
- Selfie Photo
- Permanent Address
- Current Address
- Shop Address

Seller registration must be completed before creating any Shop.

---

# Seller Verification

Every Seller must complete KYC Verification.

Successful KYC allows the Seller to create and manage multiple Shops.

KYC verification is completed only once.

Additional Shops do not require repeated KYC verification.

---

# General Authentication Rules

- Mobile Numbers must be verified using OTP.
- Every verified identity is permanently recorded.
- Authentication history is securely stored.

---

**End of Part 1**


# Rider Registration

Rider registration requires:

- Full Name
- Mobile Number
- OTP Verification
- Date of Birth
- National ID (NID) Number
- NID Front Photo
- NID Back Photo
- Selfie Photo
- Permanent Address
- Current Address

Additional requirements (if applicable):

- Driving License
- Vehicle Information

Every Rider must complete identity verification before becoming eligible for Delivery assignments.

---

# OTP Verification

OTP Verification is mandatory for:

- New Account Registration
- Mobile Number Verification
- Password Reset
- New Device Login

OTP expires automatically after the configured validity period.

Expired OTPs cannot be reused.

---

# Password Rules

Password Login is supported for:

- Customer
- Seller
- Rider

Passwords must:

- Meet minimum security requirements.
- Be securely encrypted before storage.
- Never be stored in plain text.

---

# Password Reset

If a User forgets the Password:

- OTP Verification is required.
- A new Password may be created.

The previous Password is not required during Password Reset.

---

# Google Authentication

Google Sign-In is available only for Customer Accounts.

Google authentication must use the official Google authentication service.

If a Customer later adds a Mobile Number, both Login methods remain available.

---

# Multi-Device Login

Users may log in from multiple Devices.

Every new Device requires OTP Verification before becoming trusted.

Trusted Devices remain associated with the User Account until removed.

---

# Session Management

The system securely manages authenticated Sessions.

Users may:

- View active Sessions.
- Log out from the current Device.
- Log out from all Devices.

Session activity is permanently recorded for security purposes.

---

# General Security Rules

- Authentication events are permanently logged.
- Failed authentication attempts are monitored.
- Suspicious authentication activities may be flagged for Admin Review.

---

**End of Part 2**


# Account Status

Every User Account maintains one official Account Status.

Supported Account Statuses:

- Pending Verification
- Active
- Suspended
- Blocked

Account Status changes are permanently recorded.

---

# Account Activation

A newly registered Account remains in:

- Pending Verification

After successful verification:

- The Account becomes Active.

Only Active Accounts may access marketplace services.

---

# Account Suspension

An Account may be Suspended when:

- Marketplace Policies are violated.
- Fraudulent activities are detected.
- Identity verification fails.
- Administrative investigation is required.

Suspended Accounts cannot access protected marketplace features.

---

# Account Blocking

Accounts may be Blocked only after an official administrative decision.

Blocked Accounts permanently lose marketplace access unless restored by Admin.

Every Block action must include an official reason.

---

# AI Limitations

AI acts only as an authentication assistant.

AI may:

- Detect suspicious login activity.
- Detect unusual authentication patterns.
- Flag suspicious Accounts.

AI cannot:

- Suspend Accounts.
- Block Accounts.
- Delete Accounts.
- Approve KYC Verification.

Final authority always belongs to Admin.

---

# Administrative Authority

Authorized Admins may:

- Review flagged Accounts.
- Verify identity documents.
- Approve or reject KYC.
- Suspend Accounts.
- Restore Accounts.
- Block Accounts.

Every administrative action is permanently recorded.

---

# Authentication Audit

Every authentication event is permanently stored.

Audit records include:

- User ID
- Login Method
- Device Information
- IP Address (if available)
- Authentication Result
- Date & Time

Audit records cannot be edited or deleted.

---

# Business Rules

- Every Mobile Number requires OTP Verification.
- Customer NID is optional.
- Seller and Rider KYC are mandatory.
- Seller KYC is completed only once.
- Multiple Shops do not require repeated Seller KYC.
- AI cannot make final authentication decisions.
- Authentication history is permanently stored.

---

**End of Part 3**


# Authentication Integrity

The User Authentication System must protect every User Identity through secure verification and standardized authentication procedures.

Identity fraud and unauthorized access are strictly prohibited.

---

# Privacy Protection

HAT-BAZAR protects sensitive user information.

Protected information includes:

- Mobile Number
- National ID (NID)
- Selfie Photo
- Personal Address
- Date of Birth

Only authorized Admins may access sensitive verification documents when required.

---

# Constitution Compliance

This Blueprint follows all business rules defined in:

- MASTER_PROJECT.md

If any Blueprint conflicts with this Constitution, MASTER_PROJECT.md always takes priority.

---

# Future Expansion

Future versions may introduce:

- Biometric Authentication
- Passkey Authentication
- Two-Factor Authentication (2FA)
- National Identity API Integration
- Advanced Risk-based Authentication

These features require approval from the Project Owner before implementation.

---

# Final Business Rules

- Customers may register using Google Sign-In or Mobile Number with OTP.
- Customer NID is not mandatory.
- Seller and Rider registration require full KYC verification.
- Seller KYC is completed only once.
- One verified Seller Account may manage multiple Shops.
- Every Mobile Number must be verified using OTP.
- Password Reset requires OTP verification.
- Multiple Device Login is supported.
- Every new Device requires OTP verification.
- AI may detect suspicious authentication activities.
- AI cannot approve KYC, suspend Accounts or block Accounts.
- Final authority always belongs to Admin.
- Authentication history and security audit records are permanently stored.

---

**End of User Authentication System Blueprint**

Version: 1.0

Status: Final

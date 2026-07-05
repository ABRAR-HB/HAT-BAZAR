# HAT-BAZAR Blueprint

# Wallet System

Version: 1.0

Status: Final Blueprint

---

# Purpose

The Wallet System manages all marketplace financial balances, escrow settlements, withdrawals and payment history.

The system is designed to ensure financial transparency, secure fund management and fair settlement between HAT-BAZAR, Sellers and Riders.

Customer Wallet is intentionally not supported.

---

# Scope

This Blueprint defines:

- Seller Wallet
- Rider Wallet
- Escrow System
- Settlement Rules
- Withdrawal Rules
- Refund Rules
- Wallet Security
- Wallet Transactions

---

# Wallet Types

HAT-BAZAR supports only two wallet types.

## Seller Wallet

Every Seller Account owns one Wallet.

A Seller may own multiple Shops, but all business income is managed through a single Seller Wallet.

The Wallet records earnings separately for each Shop while maintaining one combined balance.

---

## Rider Wallet

Every Rider owns one Wallet.

The Rider Wallet stores:

- Delivery Earnings
- Bonuses
- Incentives
- Withdrawal History

---

## Customer Wallet

Customer Wallet is not supported.

Customers always pay using approved external payment methods.

Examples:

- bKash
- Nagad
- Rocket
- Bank Account
- Debit Card
- Credit Card

Future payment methods may be added.

---

# Wallet Principles

The Wallet System follows these principles:

- Secure
- Transparent
- Auditable
- Traceable
- Scalable

Every financial transaction must remain permanently recorded.

---

# Escrow System

HAT-BAZAR operates as a trusted Escrow platform.

When a Customer pays for an Order:

Customer Payment

↓

HAT-BAZAR Escrow

↓

Settlement Decision

↓

Seller Wallet or Customer Refund

The Seller never receives payment immediately after Order placement.

---

**End of Part 1**


# Escrow Holding Period

After successful Delivery Confirmation, the Customer's payment remains in the HAT-BAZAR Escrow Account for three (3) days.

This period allows Customers to submit a valid Return Request according to the official Return Policy.

The Seller cannot withdraw or use the Escrow Balance during this period.

---

# Settlement Rule

If no approved Return Request is received within the three-day Escrow period:

- The payment is released automatically.
- The released amount is transferred to the Seller Wallet.
- The amount becomes Available Balance.
- The Seller may withdraw the released balance at any time.

---

# Return & Refund Rule

If an approved Return occurs within the Escrow Holding Period:

- The Product Price is refunded to the Customer.
- The refund should be sent to the same payment method used by the Customer whenever technically possible.
- The returned product goes back to the Seller.

---

# Delivery Charge Rule

Delivery Charges are not refunded after successful delivery.

Even if a Product is returned:

- HAT-BAZAR retains the Delivery Charge.
- Only the Product Price is refunded to the Customer.

Any Return Delivery Cost follows the official Return Policy.

---

# Seller Payment Release

Every released payment is transferred to the Seller Wallet.

Each released payment includes:

- Order ID
- Shop Name
- Product Name
- Gross Amount
- Released Amount
- Release Date & Time
- Settlement Status

---

# Settlement Principles

Every settlement must be:

- Transparent
- Traceable
- Secure
- Automatically Logged

Settlement records can never be deleted.

---

**End of Part 2**


# Seller Wallet Dashboard

The Seller Wallet Dashboard provides a complete financial overview for every Seller Account.

The Dashboard includes:

- Available Balance
- Escrow Balance
- Pending Release Amount
- Total Earnings
- Total Withdrawals
- Upcoming Releases

All balances are updated automatically.

---

# Available Balance

Available Balance contains only released funds.

Only Available Balance can be withdrawn.

---

# Escrow Balance

Escrow Balance contains payments that are still within the official Escrow Holding Period.

Escrow funds:

- Cannot be withdrawn.
- Cannot be transferred.
- Cannot be used for marketplace payments.

---

# Upcoming Releases

The Wallet displays all payments that will be released in the future.

For each upcoming release, the Seller can view:

- Order ID
- Shop Name
- Product Name
- Release Date
- Remaining Hold Time
- Expected Release Amount

---

# Payment Countdown

Every Escrow payment displays a live countdown.

Example:

Order ID:
HB24070125

Status:
In Escrow

Release In:
2 Days 08 Hours 14 Minutes

Expected Amount:
৳2,450

---

# Payment Status

Each payment always has one status.

Possible statuses:

- Payment Received
- In Escrow
- Pending Release
- Released
- Withdrawn
- Refunded

Status history is permanently stored.

---

# Transaction History

Every Wallet transaction is permanently recorded.

Each transaction contains:

- Transaction ID
- Date & Time
- Order ID (if applicable)
- Shop Name
- Transaction Type
- Amount
- Status
- Reference

Transaction history can never be deleted.

---

# Wallet Transparency

The Wallet System is designed to provide complete financial transparency.

Sellers can always determine:

- Which Orders have been paid.
- Which payments are still in Escrow.
- Which payments have been released.
- Which payments have been withdrawn.
- Which payments have been refunded.

---

**End of Part 3**


# Seller Withdrawal Rules

Only Available Balance can be withdrawn.

Escrow Balance can never be withdrawn.

The Seller may request withdrawal at any time.

HAT-BAZAR does not charge any Withdrawal Commission.

If the selected payment provider applies a transfer fee, only that fee is applicable.

---

# Marketplace Payments

When paying marketplace fees, the Seller chooses the payment method.

Supported payment methods include:

- Seller Wallet
- bKash
- Nagad
- Bank Account
- Debit Card
- Credit Card
- Other approved payment methods

HAT-BAZAR never deducts money automatically from the Seller Wallet.

Shop Registration Fees and Monthly Badge Charges must always be paid by the Seller through an approved payment method.

---

# Rider Wallet

Every Rider owns one Wallet.

The Rider Wallet records:

- Delivery Earnings
- Bonuses
- Incentives
- Withdrawal History

Rider earnings become available after successful delivery completion according to marketplace policy.

---

# Rider Withdrawal Policy

A Rider may submit one Withdrawal Request per calendar month.

Additional withdrawal requests require approval from the responsible Admin.

Only approved withdrawals may be processed.

---

# Wallet Security

The Wallet System maintains complete financial security.

Every transaction must be:

- Secure
- Traceable
- Auditable
- Permanently Recorded

Wallet balances can never become negative.

---

# Wallet Freeze

Only Admin may freeze or unfreeze a Wallet.

A Wallet may be frozen due to:

- Fraud Investigation
- Serious Policy Violation
- Legal Requirement

While a Wallet is frozen:

- Withdrawal is disabled.
- Transaction History remains visible.
- Existing financial records remain unchanged.

---

# AI Responsibilities

AI may assist with:

- Fraud Detection
- Suspicious Transaction Detection
- Financial Pattern Analysis
- Risk Alerts

AI cannot:

- Freeze a Wallet.
- Release Escrow Funds.
- Approve Withdrawals.
- Reverse Transactions.

Final authority always belongs to Admin.

---

# Future Scope

Future versions of the Wallet System may include:

- Multi-Currency Support
- Scheduled Withdrawals
- Financial Reports
- Automated Tax Reports
- Advanced Business Analytics

All future enhancements must comply with the HAT-BAZAR Constitution.

---

# Final Business Rules

- Customer Wallet is not supported.
- Seller Wallet and Rider Wallet are supported.
- Customer payments are held in HAT-BAZAR Escrow.
- Seller receives payment only after the Escrow Holding Period ends.
- Approved Returns refund only the Product Price.
- Delivery Charges are retained by HAT-BAZAR.
- Sellers may withdraw only Available Balance.
- HAT-BAZAR charges no Sales Commission.
- HAT-BAZAR charges no Withdrawal Commission.
- Seller Wallet funds are never deducted automatically.
- Riders may withdraw once per month unless additional withdrawals are approved by Admin.

---

**End of Wallet System Blueprint**

Version: 1.0

Status: Final

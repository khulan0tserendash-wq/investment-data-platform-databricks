# Conceptual Data Model

## Purpose

This conceptual model represents the main business concepts required by the investment data platform.

At this stage, the model focuses on business entities and relationships rather than technical implementation details.

---

## Core Business Entities

### Issuer

A legal entity that issues financial instruments.

Examples:

- Corporation
- Government
- Financial institution

---

### Security

A financial instrument that can be held in an investment portfolio.

Examples:

- Equity
- Bond
- Future
- Swap

---

### Security Identifier

An identifier used to identify a security across financial systems or data vendors.

Examples:

- ISIN
- CUSIP
- SEDOL
- Bloomberg identifier

---

### Portfolio

A collection of investments managed together.

---

### Position

Represents a portfolio's holding in a particular security on a specific date.

---

### Price

Represents the market price of a security at a particular point in time.

---

### Price Source

Represents the source or vendor that supplied a price.

Examples:

- Vendor A
- Vendor B
- Internal pricing source

Real vendor data is not used in this project.

---

### Currency

Represents the currency associated with securities, prices, and portfolios.

Examples:

- USD
- EUR
- GBP

---

## Conceptual Relationships

```mermaid
erDiagram

    ISSUER ||--o{ SECURITY : issues

    SECURITY ||--o{ SECURITY_IDENTIFIER : has

    SECURITY ||--o{ PRICE : has

    PRICE_SOURCE ||--o{ PRICE : provides

    PORTFOLIO ||--o{ POSITION : contains

    SECURITY ||--o{ POSITION : held_as

    CURRENCY ||--o{ SECURITY : denominates

    CURRENCY ||--o{ PORTFOLIO : base_currency

    CURRENCY ||--o{ PRICE : price_currency

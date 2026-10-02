# Logical Data Model

## Purpose

This logical data model defines the main entities, attributes, keys, and relationships for the investment data platform.

The logical model is independent of the physical Databricks implementation.

---

# 1. Issuer

Represents a legal entity that issues financial instruments.

## Attributes

| Attribute | Description |
|---|---|
| issuer_id | Internal unique identifier for the issuer |
| legal_name | Legal name of the issuing entity |
| lei | Legal Entity Identifier |
| country_code | Country of incorporation |
| issuer_type | Classification of issuer |

## Key

**Primary Key:** `issuer_id`

## Example Issuer Types

- Corporate
- Financial Institution
- Government
- Supranational

---

# 2. Security

| Attribute | Description |
|---|---|
| security_id | Internal unique identifier |
| issuer_id | Optional issuer associated with the security |
| instrument_type_code | Classification of the financial instrument |
| security_name | Human-readable security name |
| currency_code | Primary denomination currency |
| issue_date | Date the security was issued |
| maturity_date | Date the instrument matures, if applicable |

## Keys

**Primary Key:** `security_id`

**Foreign Keys:**

- `issuer_id` → Issuer (optional)
- `instrument_type_code` → Instrument Type
- `currency_code` → Currency
---

# 3. Security Identifier

Stores external identifiers associated with a security.

## Attributes

| Attribute | Description |
|---|---|
| security_identifier_id | Internal unique identifier |
| security_id | Security associated with the identifier |
| identifier_type | Type of identifier |
| identifier_value | Identifier value |
| valid_from | Date identifier becomes valid |
| valid_to | Date identifier stops being valid |

## Keys

**Primary Key:** `security_identifier_id`

**Foreign Key:**

- `security_id` → Security

## Example Identifier Types

- ISIN
- CUSIP
- SEDOL
- Bloomberg ID

---

# 4. Currency

Represents currencies used by the platform.

## Attributes

| Attribute | Description |
|---|---|
| currency_code | ISO currency code |
| currency_name | Currency name |

## Key

**Primary Key:** `currency_code`

## Examples

- USD
- EUR
- GBP
- JPY

---
# 5. Instrument Type

Represents the controlled classification of financial instruments.

## Attributes

| Attribute | Description |
|---|---|
| instrument_type_code | Unique instrument-type code |
| instrument_type_name | Human-readable instrument type |
| description | Description of the instrument category |

## Key

**Primary Key:** `instrument_type_code`

## Examples

- EQUITY
- BOND
- FUTURE
- SWAP

---

# 6. Portfolio

| Attribute | Description |
|---|---|
| portfolio_id | Internal unique identifier |
| portfolio_code | Business identifier for the portfolio |
| portfolio_name | Portfolio name |
| base_currency_code | Portfolio reporting currency |
| inception_date | Date the portfolio was created |

## Keys

**Primary Key:** `portfolio_id`

**Business Key:** `portfolio_code`

**Foreign Key:**

- `base_currency_code` → Currency

# 7. Position

Represents a portfolio holding in a security on a specific valuation date.

## Attributes

| Attribute | Description |
|---|---|
| position_id | Internal unique identifier |
| portfolio_id | Portfolio holding the security |
| security_id | Security being held |
| position_date | Valuation date |
| quantity | Quantity held |
| market_value | Market value of the position |
| market_value_currency | Currency of market value |

## Keys

**Primary Key:** `position_id`

**Foreign Keys:**

- `portfolio_id` → Portfolio
- `security_id` → Security
- `market_value_currency` → Currency

## Business Grain

One record represents:

**one security held by one portfolio on one valuation date**

---

# 8. Price Source

Represents a provider or source of market prices.

## Attributes

| Attribute | Description |
|---|---|
| price_source_id | Internal unique identifier |
| source_name | Name of pricing source |
| source_type | Type of pricing source |

## Key

**Primary Key:** `price_source_id`

---

# 9. Price

Represents a market price for a security.

## Attributes

| Attribute | Description |
|---|---|
| price_id | Internal unique identifier |
| security_id | Security being priced |
| price_source_id | Source providing the price |
| price_date | Pricing date |
| price_value | Price value |
| price_currency | Currency of the price |

## Keys

**Primary Key:** `price_id`

**Foreign Keys:**

- `security_id` → Security
- `price_source_id` → Price Source
- `price_currency` → Currency

  # Business Keys and Uniqueness Rules

## Issuer

Technical Primary Key:

`issuer_id`

Business Identifier:

`lei`, when available.

---

## Portfolio

Technical Primary Key:

`portfolio_id`

Business Key:

`portfolio_code`

---

## Position

Technical Primary Key:

`position_id`

Business uniqueness for the current model:

`portfolio_id + security_id + position_date`

---

## Price

Technical Primary Key:

`price_id`

Business uniqueness for the current model:

`security_id + price_source_id + price_date + price_currency`

---

# Optionality Decisions

## Security and Issuer

`issuer_id` is optional.

Some instrument types, such as swaps and certain derivatives, may not have a single issuer in the same sense as equities or bonds.

The model will be refined further when instrument subtypes are introduced.

## Business Grain

One record represents:

**one price for one security, from one source, on one pricing date**

---

# Logical Relationships

```mermaid
erDiagram

    ISSUER ||--o{ SECURITY : issues
    SECURITY ||--o{ SECURITY_IDENTIFIER : has
    SECURITY ||--o{ PRICE : priced_by
    PRICE_SOURCE ||--o{ PRICE : provides
    PORTFOLIO ||--o{ POSITION : contains
    SECURITY ||--o{ POSITION : held_as
    CURRENCY ||--o{ SECURITY : denominates
    CURRENCY ||--o{ PORTFOLIO : base_currency
    CURRENCY ||--o{ POSITION : valuation_currency
    CURRENCY ||--o{ PRICE : price_currency


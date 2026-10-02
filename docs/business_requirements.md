# Business Requirements

## Project Overview

This project models a simplified enterprise investment data platform for an asset management company.

The platform will centralize information about financial instruments, issuers, portfolios, positions, prices, and reference data.

The project is designed to demonstrate:

- conceptual data modelling
- logical data modelling
- physical data modelling
- relational modelling
- dimensional modelling
- Databricks and Delta Lake
- SQL and PySpark
- data quality controls
- data lineage
- medallion architecture

All data used in this project is synthetic.

---

## Business Problem

An investment management company receives financial instrument and portfolio data from multiple source systems.

The company needs a centralized data platform that provides consistent and reliable information about:

- financial instruments
- issuers
- portfolios
- positions
- prices
- currencies
- security identifiers

The platform must support both operational data management and analytical reporting.

---

## Core Business Requirements

The platform must:

1. Maintain information about financial instruments.

2. Support multiple instrument types, including:
   - Equity
   - Bond
   - Future
   - Swap

3. Maintain issuer information.

4. Support multiple identifiers for a financial instrument, such as:
   - ISIN
   - CUSIP
   - SEDOL
   - Bloomberg identifiers

5. Maintain historical market prices.

6. Support multiple price sources or vendors.

7. Maintain investment portfolios.

8. Store portfolio positions by valuation date.

9. Support multiple currencies.

10. Maintain relationships between securities and issuers.

11. Validate critical data attributes.

12. Detect missing or invalid relationships between datasets.

13. Maintain traceability from raw source data to curated analytical datasets.

14. Support investment reporting and analytics.

---

## Initial Business Entities

The initial model will contain the following business entities:

- Issuer
- Security
- Security Identifier
- Instrument Type
- Bond
- Future
- Swap
- Currency
- Price
- Price Source
- Portfolio
- Position
- Date

---

## Out of Scope — Initial Version

The first version will not include:

- real Bloomberg data
- proprietary financial institution data
- trading execution
- accounting
- settlement
- corporate actions
- derivatives valuation models
- market risk calculations

These areas may be added in later versions.

---

## Data Privacy and Confidentiality

All datasets, company names, instrument identifiers, portfolios, prices, and transactions used in this repository are synthetic.

No proprietary information from any current or previous employer is used.

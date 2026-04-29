# Audit Logic Documentation

**Project:** CDD Data Quality Review  
**Author:** Britney C. Clark  
**Purpose:** Plain-language explanation of all exception rules for non-technical stakeholders

---

## What This Document Is For

When an audit team performs data analytics, every decision made in the code — why a record was flagged, what rule was applied, what threshold was used — needs to be documented so that:
1. A reviewer can understand the logic without reading the code
2. The methodology can be defended if findings are questioned
3. The same routine can be reused or updated in future audit cycles

This document serves that purpose for the CDD Data Quality Review project.

---

## Data Sources

| Source | Description | Format |
|---|---|---|
| `raw/cdd_records.csv` | Synthetic customer CDD profiles | CSV |

All data is synthetically generated for portfolio demonstration purposes.

---

## Exception Rule Definitions

### EX-01 — Missing Required Fields
**Rule:** Flag any record where one or more of the following fields is null or blank:
- `customer_name`
- `date_of_birth`
- `ssn_tin` (Tax Identification Number)
- `account_open_date`
- `cdd_collection_date`

**Why it matters:** Incomplete CDD profiles indicate a control gap. Regulators require that all required identifying information be collected at account opening.

---

### EX-02 — Invalid Age
**Rule:** Flag any record where the calculated age (today's date minus `date_of_birth`) is less than 18 years or greater than 120 years.

**Why it matters:** Ages outside this range almost certainly indicate a data entry error or a system integrity issue. A customer under 18 should not hold certain account types; ages over 120 are statistically implausible.

---

### EX-03 — Duplicate Customer Records
**Rule:** Flag any set of records that share the same `ssn_tin` but have different `customer_id` values.

**Why it matters:** Duplicate identities in a system can indicate that the same customer was onboarded multiple times, potentially with different risk profiles assigned. This is a data integrity and control gap.

---

### EX-04 — Account Open Date Before CDD Collection Date
**Rule:** Flag any record where `account_open_date` is earlier than `cdd_collection_date`.

**Why it matters:** CDD information should be collected at or before account opening. If an account was opened before CDD was collected, it means the customer was onboarded without required information on file — a sequencing failure in the control process.

---

### EX-05 — Stale CDD Record
**Rule:** Flag any record where the number of days since `cdd_last_refreshed` exceeds the required review window:
- Standard risk customers: 1,825 days (5 years)
- High-risk customers: 365 days (1 year)

**Why it matters:** BSA/AML regulations require that customer profiles be periodically reviewed and updated. Stale records suggest the refresh control is not operating effectively.

---

### EX-06 — High-Risk Flag Without Enhanced Due Diligence
**Rule:** Flag any record where `risk_rating` = "HIGH" and `enhanced_due_diligence_on_file` = False/null.

**Why it matters:** High-risk customers require Enhanced Due Diligence (EDD) under AML regulations. A high-risk flag without corresponding EDD documentation is a direct control failure and a potential regulatory finding.

---

## How Exceptions Are Reported

Each flagged record is written to `outputs/exception_report.csv` with:
- The original record fields
- An `exception_code` column listing all applicable codes (comma-separated if multiple apply)
- An `exception_count` column indicating how many rules were triggered

Population-level summary statistics (total records, exception counts by code, exception rate) are written to `outputs/summary_stats.xlsx` for use in audit workpaper documentation.

---

## Limitations and Assumptions

- All data is synthetic — thresholds and rules are illustrative, not derived from any real institution's policies
- Risk rating logic is simplified — real-world risk models are more complex
- This project does not replicate any proprietary system or internal data from LPL Financial

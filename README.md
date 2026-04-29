# Audit Analytics Portfolio — Customer Due Diligence (CDD) Data Quality Review

**Author:** Britney C. Clark  
**Tools:** Python · pandas · SQL · Alteryx · Excel  
**Status:** 🔄 In Progress — actively building

---

## Overview

This project demonstrates a data analytics workflow designed to support an Internal Audit team in performing **full-population testing and anomaly detection** on Customer Due Diligence (CDD) data — the kind of data financial institutions must maintain to comply with the **Bank Secrecy Act (BSA)** and Anti-Money Laundering (AML) regulations.

The workflow simulates the end-to-end process an audit data analyst would follow:
1. Ingest and profile raw customer data
2. Clean and validate for completeness and consistency
3. Apply exception rules to flag anomalies
4. Summarize and document findings in a format suitable for audit workpapers

> **Note:** All data used in this project is synthetically generated and does not represent any real individuals or institutions.

---

## Business Context

Broker-dealers and RIAs are required under BSA/AML regulations to collect, maintain, and periodically refresh Customer Due Diligence information. Internal Audit teams test these controls by analyzing the **full population** of customer records — not just a sample — to identify gaps, inconsistencies, or missing data that could indicate a control failure.

This project simulates that audit process at scale.

---

## Project Structure

```
audit-analytics-portfolio/
│
├── data/
│   ├── raw/                    # Synthetic source data (as-received)
│   └── processed/              # Cleaned, validated datasets
│
├── scripts/
│   ├── 01_data_profiling.py    # Initial data discovery & summary stats
│   ├── 02_data_cleaning.py     # Standardization, null handling, type validation
│   ├── 03_exception_testing.py # Rule-based anomaly and exception detection
│   └── 04_reporting.py         # Summary output for audit documentation
│
├── notebooks/
│   └── exploratory_analysis.ipynb   # EDA and visualization (in progress)
│
├── outputs/
│   ├── exception_report.csv    # Flagged records with exception codes
│   └── summary_stats.xlsx      # Population-level summary for workpaper
│
├── docs/
│   └── audit_logic.md          # Plain-language documentation of all rules
│
└── README.md
```

---

## Audit Test Logic (Exception Rules)

The following exception categories are tested across the full population:

| Exception Code | Description | Audit Relevance |
|---|---|---|
| `EX-01` | Missing or null required fields (name, DOB, SSN/TIN) | Incomplete CDD profile |
| `EX-02` | Date of birth implies customer age < 18 or > 120 | Data integrity issue |
| `EX-03` | Duplicate customer records (same SSN/TIN, different ID) | Identity control gap |
| `EX-04` | Account open date precedes CDD collection date | Sequencing anomaly |
| `EX-05` | CDD record not refreshed within required review window | Stale profile — regulatory risk |
| `EX-06` | High-risk flag set with no enhanced due diligence on file | Control failure indicator |

---

## Skills Demonstrated

- **Full-population testing** using Python (pandas) — no sampling, every record evaluated
- **SQL-to-Python translation** — logic designed to mirror the kind of queries I've written in Snowflake
- **Data validation and reconciliation** — completeness, consistency, referential integrity
- **Anomaly detection** using rule-based exception logic
- **Audit documentation** — logic documented in plain language for non-technical stakeholders
- **Repeatable, modular scripts** — each stage of the pipeline is independently runnable (automation-ready)
- **Visualization** — summary charts for stakeholder reporting (notebooks, in progress)
- **AI-assisted development** — using Claude to get code feedback, understand Python patterns, and accelerate learning

---

## Background

During my internship with LPL Financial's Internal Audit Data Analytics team (June–December 2024), I supported a live AML/CDD audit by writing SQL queries against Snowflake databases, building Alteryx workflows for data transformation, and documenting exceptions in structured workpapers. That experience directly informed the design of this project.

This portfolio is my attempt to rebuild a version of that workflow in Python — both to deepen my programming skills and to demonstrate the analytical thinking behind the audit process. I'm also using AI tools (primarily Claude) to accelerate my learning: getting feedback on my code, understanding Python patterns, and translating concepts I already know from SQL and Alteryx into Python equivalents.

The goal is to show not just that I can follow a tutorial, but that I understand *why* the audit logic works — because I've applied it in a real environment.

---

## Roadmap

- [x] Define project structure and audit logic
- [x] Document exception rules and business context
- [ ] Generate synthetic CDD dataset (script in progress)
- [ ] Complete `01_data_profiling.py`
- [ ] Complete `02_data_cleaning.py`
- [ ] Complete `03_exception_testing.py`
- [ ] Build summary output and exception report
- [ ] Add exploratory notebook with visualizations
- [ ] Finalize `docs/audit_logic.md`

---

## Contact

[LinkedIn](https://linkedin.com/in/britneyc-clark) · BritneyC.Clark@gmail.com

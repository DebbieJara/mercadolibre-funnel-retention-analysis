# MercadoLibre Funnel & Retention Analysis (SQL)

![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat&logo=postgresql&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-336791?style=flat&logo=postgresql&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-34A853?style=flat&logo=google-sheets&logoColor=white)

**Business question:** MercadoLibre's product director posed a challenge every growth team eventually faces: at which stage do we lose users, and how can we improve their retention over time? Millions of users browse, click, and abandon. The question isn't whether drop-off happens, it's where, how much, and what to do about it.

## Context

I used SQL to find the answers.

## Process

Using two datasets covering January-August 2025, I mapped the complete conversion funnel from first visit to purchase, identified the largest drop-off points, and analyzed user retention at D7, D14, D21, and D28, both by country and by monthly cohort. Every metric was built from scratch in SQL using CTEs, window functions, and conditional aggregation. Results were exported to Google Sheets for visualization and executive reporting.

## Key findings

### Funnel

- The largest drop-off occurs at the select_item → add_to_cart step, with a \~65.9% loss, signalling critical friction at purchase intent rather than at checkout.
- Conversion from select_item to add_to_cart is \~14% overall.
- Drop-off varies by country: Uruguay and Chile retain more users at this stage; Peru and Bolivia show the highest losses.
- The problem is not transactional. It is rooted in trust, perceived value, and information clarity.

### Retention

- Initial retention is strong: D7 \~86%, but falls sharply to \~2-3% by D28, revealing a habit-formation gap.
- Cohorts January-July show stable behavior across all retention points.
- The August 2025 cohort is anomalous: D7 dropped to 70.8% and D28 to just 0.2%, suggesting a deterioration in acquisition quality or onboarding experience that requires immediate investigation.

## Dashboard

![Executive summary dashboard](images/dashboard_resumen.png)

Executive summary dashboard built in Google Sheets.

![Conversion funnel by stage](images/funnel_general.png)

Overall conversion funnel: drop-off at each stage.

![Drop-off by country](images/funnel_por_pais.png)

select_item → add_to_cart drop-off segmented by country.

![Retention curves by cohort](images/retencion_cohortes.png)

D7-D28 retention curves by monthly cohort. August anomaly visible.

## Technical details

### Dataset

| Table | Description |
|---|---|
| mercadolibre_funnel | User events during the purchase process (first_visit, select_item, add_to_cart, begin_checkout, add_shipping_info, add_payment_info, purchase) |
| mercadolibre_retention | Recurring activity by user and period (signup date, activity date, active flag, days after signup) |

Period analyzed: January 1 - August 31, 2025

### Analytical workflow

| Step | Description |
|---|---|
| 1. Data exploration | Reviewed available events and table structure |
| 2. General funnel | Multi-stage funnel using CTEs and LEFT JOINs; conversion rate at each stage relative to first visits |
| 3. Funnel by country | Segmented all funnel stages by country to identify geographic drop-off patterns |
| 4. Retention by country | Calculated D7, D14, D21, D28 retention counts and percentages per country |
| 5. Cohort retention | Assigned each user to a monthly cohort based on signup date and tracked retention over time |

### Key SQL techniques

- Multi-stage CTE funnels with LEFT JOIN chaining
- NULLIF to avoid division by zero in conversion calculations
- Conditional aggregation with CASE WHEN for retention metrics
- DATE_TRUNC and TO_CHAR for cohort month assignment
- COUNT(DISTINCT user_id) for accurate user deduplication

## Tools

SQL (PostgreSQL) · Google Sheets

## Repository structure

mercadolibre-funnel-retention-analysis/
├── README.md
├── mercadolibre_analysis.sql
└── images/
    ├── dashboard_resumen.png
    ├── funnel_general.png
    ├── funnel_por_pais.png
    └── retencion_cohortes.png

## Dataset

Full analysis (Google Sheets): [View executive summary, funnel analysis, and retention analysis by country and cohort](https://docs.google.com/spreadsheets/d/13A1IXezYw4e-keuND5KnZpFjUv5mq9yZ/edit?gid=1401846721#gid=1401846721)

---

By Deborah Jara | Business Intelligence · Data Analytics | Mexico
[LinkedIn](https://www.linkedin.com/in/deborahjara) · [GitHub](https://github.com/DebbieJara)

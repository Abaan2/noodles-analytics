
# Noodles Crypto Analytics Platform

Dashboard screenshot: docs/screenshots/executive-dashboard.png

## Project Overview

Production-ready currency analytics platform processing 1M+ rows of
market data daily using Python, MySQL, and Power BI.

Key Features:
- Automated Python ETL pipeline (5-min runtime)
- Star schema data warehouse design
- Interactive Power BI dashboards
- Comprehensive data quality validation
- Scheduled daily refresh

## Architecture

json Files → Python ETL → MySQL (noodles_dw) → Power BI Dashboards

Architecture diagram (YOU CREATE in Task 9 §1):
docs/architecture-diagram.png

## Technology Stack

| Layer | Technology |
| ETL | Python 3.9, pandas, SQLAlchemy |
| Data Warehouse | MySQL 8.x |
| Visualization | Power BI Desktop, DAX |

## Project Structure

noodles-analytics/
├── 05_data_warehouse_design.ipynb
├── 06_powerbi_prep.ipynb
├── reports/          (Tasks 7–8 .pbix files)
├── docs/             ← YOU CREATE in Task 9
│   ├── architecture-diagram.png    ← §1
│   ├── data-dictionary.xlsx        ← §2
│   ├── technical-runbook.md        ← §3
│   ├── user-guide.md               ← §4
│   └── demo-presentation.pptx      ← §5
└── README.md

## Data Model

Dimensions: DimCurrency, DimDate, DimPlatform
Facts: FactSocialEngagement

Data dictionary (YOU CREATE in Task 9 §2):
docs/data-dictionary.xlsx

## Key Insights

- Bitcoin Dominance: 45% of total market capitalization
- Volume Leaders: Top 20 currencies = 90% of trading volume
- Social Correlation: Engagement predicts 30% of price movements
- Volatility: Average daily price change of ±8%

## Achievements

- Processed 1M+ rows of currency data
- Built automated ETL pipeline
- Delivered interactive Power BI dashboards
- Referential integrity in star schema

## Documentation  (files YOU CREATE — not pre-built reference links)

- Technical Runbook  →  docs/technical-runbook.md       (Task 9 §3)
- User Guide         →  docs/user-guide.md              (Task 9 §4)
- Architecture       →  docs/architecture-diagram.png   (Task 9 §1)
- Data Dictionary    →  docs/data-dictionary.xlsx       (Task 9 §2)

## Demo

Demo video URL: https://youtu.be/your-video-id
Presentation (YOU CREATE in Task 9 §5): docs/demo-presentation.pptx

## Contact

Your Name | your.email@example.com

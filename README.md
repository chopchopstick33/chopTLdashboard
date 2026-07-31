# chopTLdashboard

Tesla ML & Automation — a solo learning project building an end-to-end data and machine learning pipeline from my own Tesla, using Fleet API telemetry and USB dashcam footage.

## What this is

Two raw data sources (Fleet Telemetry + dashcam clips) flow into ingestion scripts, land in structured storage (Postgres/TimescaleDB) and object storage, feed parallel ETL/feature and CV/ML processing tracks, and converge into a scheduled cloud pipeline with a dashboard on top. The project is treated like a small solo startup: documentation and schema before code, small shippable increments, and cloud deployment planned from the start.

Goals: learn ML, MLOps, data engineering, and automation end-to-end — from writing the project brief and drawing the ERD, through pipelines and model training, to deployment, testing, and monitoring.

## Roadmap

Progress is milestone-based (no fixed calendar). See the full plan in [`docs/PROJECT_PLAN.md`](docs/PROJECT_PLAN.md).

| Milestone | Focus | Status |
|---|---|---|
| 0 | Foundations & environment | 🔄 In progress |
| 1 | Telemetry ingestion (Fleet API) | ⬜ Not started |
| 2 | Dashcam ingestion pipeline | ⬜ Not started |
| 3 | Unified schema & warehouse | ⬜ Not started |
| 4 | EDA & baseline ML | ⬜ Not started |
| 5 | Computer vision on dashcam footage | ⬜ Not started |
| 6 | Automation & orchestration | ⬜ Not started |
| 7 | Cloud deployment | ⬜ Not started |
| 8 | Testing, monitoring & polish | ⬜ Not started |

## Tech stack (planned)

Python · Tesla Fleet API / Fleet Telemetry · PostgreSQL + TimescaleDB · OpenCV + YOLO · scikit-learn / XGBoost / PyTorch · Prefect · MLflow · Docker · Terraform · GitHub Actions · Streamlit or Next.js

Full stack rationale and per-layer choices are in the [project plan](docs/PROJECT_PLAN.md#recommended-tech-stack).

## Repository structure (planned)

```
chopTLdashboard/
├── docs/            # Project plan, ADRs, data dictionary, engineering journal
├── ingestion/       # Fleet API telemetry + dashcam clip ingestion
├── etl/             # Cleaning, joins, feature engineering
├── ml/              # Notebooks, training, evaluation
├── pipelines/       # Prefect flows / orchestration
├── dashboard/       # Streamlit or Next.js app
└── tests/           # pytest suites
```

## Task tracking

Day-to-day tasks live on the Trello board: [Tesla ML & Automation Project](https://trello.com/b/BCEDjDCF/tesla-ml-automation-project)
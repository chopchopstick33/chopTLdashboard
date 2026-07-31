# Tesla Automotive ML & Automation — Solo Project Plan

A milestone-based roadmap (no fixed calendar — move at your own pace) for learning ML, MLOps, and automation end-to-end using your own Tesla's Fleet API telemetry and dashcam footage. Treated like a real solo startup: docs and schema before code, small shippable increments, cloud from early on rather than bolted on at the end.

---

## Guiding principles

- **Ship at every milestone.** Each stage produces something real and running — not just notes.
- **Docs and schema before code.** Write the one-pager or draw the ERD before opening an editor.
- **Small increments.** A working v0.1 pipeline beats a half-built v2.
- **Cloud from the start, cheaply.** Use free tiers locally-simulated first, promote to real cloud once the shape is proven — don't over-engineer infra before you have data flowing.

---

## Recommended tech stack

| Layer | Tools |
|---|---|
| Language | Python (core), optionally TypeScript for a dashboard |
| Telemetry ingestion | Tesla Fleet API + Fleet Telemetry, `tesla-fleet-api` (PyPI) or direct REST calls |
| Video ingestion | USB dashcam export (manual or scripted sync), OpenCV for frame handling |
| Structured storage | PostgreSQL + TimescaleDB extension (great fit for time-series telemetry) |
| Object storage | Local filesystem → S3 / GCS / Azure Blob once cloud-ready |
| Orchestration | Prefect (friendlier solo-dev learning curve than Airflow) |
| ML | scikit-learn / XGBoost for tabular baselines, PyTorch for CV |
| Computer vision | OpenCV, YOLOv8/YOLOv11 for object detection on dashcam frames |
| Experiment tracking | MLflow |
| Data versioning | DVC (optional, once datasets get large) |
| Data validation | Pandera or Great Expectations |
| Containerization | Docker |
| IaC | Terraform (once the manual cloud setup works) |
| CI/CD | GitHub Actions |
| Testing | pytest |
| Monitoring | Grafana + Prometheus, or your cloud provider's native dashboards |
| Dashboard | Streamlit (fast) or Next.js (if you want frontend practice too) |

**On picking a cloud provider:** since the goal is learning cloud broadly, any of AWS, GCP, or Azure works — pick one and commit rather than splitting effort. AWS has the most transferable/industry-standard skills and documentation. GCP's free tier and BigQuery are especially friendly for a data-heavy hobby project. Azure is a reasonable choice too, especially if you want exposure to the Microsoft ecosystem. If you're unsure, GCP or AWS free tier is a safe default to start Milestone 0 with.

---

## Roadmap

### Milestone 0 — Foundations & environment
- Create a Tesla Developer account, register a Fleet API application, generate the public/private key pair
- Set up a GitHub repo: README, project structure, license, `.gitignore`
- Set up local Python environment (`uv` or `poetry`), linting, pre-commit hooks
- Write a one-page **project brief**: problem statement, goals, non-goals, success criteria
- Pick your cloud provider and create a free-tier account — don't build on it yet, just have it ready

### Milestone 1 — Telemetry ingestion (Fleet API)
- Get OAuth working end-to-end against your own vehicle
- Pull one-off `vehicle_data` snapshots; study the payload shape
- Configure Fleet Telemetry streaming
- Draft v1 of the DB schema: `drives`, `telemetry_points`, `charging_sessions`
- Store both raw JSON payloads and parsed relational rows
- Write a data dictionary doc describing every field you're capturing

### Milestone 2 — Dashcam ingestion pipeline
- Script to detect and copy new USB dashcam clips
- Parse Tesla's clip naming/folder convention (front/rear/side repeater cameras, event type)
- Extract metadata: timestamp, event type (Sentry / Saved / user-triggered)
- Store video files + metadata, linked to a `video_clips` table tied to trips/events
- Basic preprocessing: frame extraction, resizing, thumbnailing

### Milestone 3 — Unified data schema & warehouse
- Draw the full ERD tying telemetry, video, and trips together
- Migrate ingestion into a proper Postgres/Timescale schema (local first)
- Add a data validation layer (Pandera / Great Expectations)
- Write ETL scripts: raw → cleaned → feature-ready tables

### Milestone 4 — Exploratory analysis & baseline ML
- Notebook-based EDA: driving patterns, energy consumption, charging behavior
- Pick one well-scoped ML problem from the idea menu below
- Build a baseline model (regression/classification) with scikit-learn
- Track experiments in MLflow from day one — build the habit early

### Milestone 5 — Computer vision on dashcam footage
- Start with a pretrained model (YOLO) run against your own clips
- Build a small labeled dataset from your own footage for one specific task
- Fine-tune / evaluate on your data
- Combine CV outputs (detected objects, events) with telemetry as joint features

### Milestone 6 — Automation & orchestration
- Move from manual scripts to scheduled Prefect flows
- Automate: telemetry polling, dashcam sync, feature computation, model retraining
- Add logging and basic alerting (failed run notifications)

### Milestone 7 — Cloud deployment
- Containerize everything with Docker
- Move storage to the cloud (S3/GCS/Blob + managed Postgres/Timescale)
- Deploy pipelines to a cloud scheduler (AWS Step Functions/Lambda, GCP Cloud Run + Scheduler, or Azure Functions)
- Introduce Terraform once the manual setup is proven
- Set up CI/CD with GitHub Actions

### Milestone 8 — Testing, monitoring, polish
- Unit tests for ETL and feature code (pytest)
- Data quality checks built into the pipeline itself
- Model evaluation dashboards
- A small web dashboard (Streamlit or Next.js) visualizing your own driving, energy, and detection data

---

## Project idea menu

Pick one or two of these to anchor Milestones 4–5 — don't try to do all of them at once:

- **Energy/efficiency predictor** — predict Wh/km for a planned trip from weather, route, and driving history
- **Battery degradation model** — track capacity over time, project future range
- **Personal driving score** — combine acceleration/braking telemetry into a smoothness/efficiency score
- **Charging optimizer** — recommend charge windows based on your patterns + off-peak electricity pricing
- **Dashcam object detector** — flag cyclists/pedestrians or near-misses from Sentry clips
- **Trip auto-classifier** — commute vs. errand vs. road trip, from telemetry alone
- **Sentry event summarizer** — daily digest of Sentry events, using CV to filter false positives (leaves, shadows) from real ones

---

## Documentation practice to build as you go

- README per module/milestone
- **ADRs** (architecture decision records) — short dated markdown files for key decisions, e.g. "why Postgres over a NoSQL store"
- A living data dictionary
- A running engineering journal (dated entries) — doubles as a great portfolio narrative later

## Testing strategy

- `pytest` for pipeline and ETL code
- Data contract tests (schema/range checks) at ingestion
- Model evaluation harness with a fixed holdout set, metrics logged in MLflow
- Smoke tests on the deployed pipeline — confirm the scheduled job actually produced fresh data

---

## Suggested first three concrete actions

1. Register a Tesla Developer app and get one successful authenticated API call against your own VIN
2. Sketch the first ERD (`drives` / `telemetry_points` / `charge_sessions` / `video_clips`) — pen and paper is fine
3. Write the project brief doc: problem, goals, non-goals, success criteria

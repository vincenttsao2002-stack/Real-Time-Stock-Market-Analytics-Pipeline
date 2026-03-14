# Real-Time-Stock-Market-Analytics-Pipeline
![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8?logo=snowflake&logoColor=white)
![DBT](https://img.shields.io/badge/dbt-FF694B?logo=dbt&logoColor=white)
![Apache Airflow](https://img.shields.io/badge/Apache%20Airflow-017CEE?logo=apacheairflow&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![Kafka](https://img.shields.io/badge/Apache%20Kafka-231F20?logo=apachekafka&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?logo=powerbi&logoColor=black)

---

> End-to-end real-time data pipeline using the Modern Data Stack. Captures live stock market data, streams in real time, orchestrates transformations, and delivers analytics-ready insights — all in one unified project.

---

## Tech Stack

| Tool | Role |
|------|------|
| **Snowflake** | Cloud Data Warehouse |
| **DBT** | SQL-based Transformations |
| **Apache Airflow** | Workflow Orchestration |
| **Apache Kafka** | Real-time Streaming |
| **Python** | Data Fetching & API Integration |
| **Docker** | Containerization |
| **Power BI** | Data Visualization |

---

## Key Features

```
01  Live stock market data via Finnhub API — not simulated
02  Real-time streaming pipeline powered by Kafka
03  Orchestrated ETL workflow using Airflow DAGs
04  Bronze → Silver → Gold transformations via DBT
05  Scalable cloud warehouse on Snowflake
06  Analytics-ready Power BI dashboards
```

---

## Repository Structure

```text
real-time-stocks-pipeline/
├── producer/                     # Kafka producer (Finnhub API)
│   └── producer.py
├── consumer/                     # Kafka consumer (MinIO sink)
│   └── consumer.py
├── dbt_stocks/models/
│   ├── bronze/
│   │   ├── bronze_stg_stock_quotes.sql
│   │   └── sources.yml
│   ├── silver/
│   │   └── silver_clean_stock_quotes.sql
│   └── gold/
│       ├── gold_candlestick.sql
│       ├── gold_kpi.sql
│       └── gold_treechart.sql
├── dag/
│   └── minio_to_snowflake.py
├── docker-compose.yml            # Kafka, Zookeeper, MinIO, Airflow, Postgres
├── requirements.txt
└── README.md
```

---

## Getting Started

1. Clone this repo and set up your environment
2. Start Kafka + Airflow services via Docker
3. Run the Python producer to fetch live stock data
4. Data flows into Snowflake — DBT applies transformations
5. Orchestrate everything with Airflow
6. Connect Power BI for live visualization

---

## Implementation

### 01 — Kafka Setup
Configured Apache Kafka locally using Docker. Created a `stocks-topic` to handle live stock market events. Defined producers (API fetch) and consumers (pipeline ingestion).

---

### 02 — Live Market Data Producer
Python producer script fetches real-time stock prices from the **Finnhub API** and streams them into Kafka in JSON format.
→ [`producer/producer.py`](producer/producer.py)

---

### 03 — Kafka Consumer → MinIO
Consumer reads streaming data from Kafka and stores it in **MinIO buckets** (S3-compatible storage), organized into folders for the raw/bronze layer.
→ [`consumer/consumer.py`](consumer/consumer.py)

---

### 04 — Airflow Orchestration
Airflow initialized in Docker. DAG loads data from MinIO into Snowflake staging tables (Bronze layer) on a **1-minute schedule**.
→ [`dag/minio_to_snowflake.py`](dag/minio_to_snowflake.py)

---

### 05 — Snowflake Warehouse Setup
Created Snowflake database, schema, and warehouse. Defined staging tables for the full Bronze → Silver → Gold layer architecture.
→ [`snowflake/sql_init.sql`](snowflake/sql_init.sql)

---

### 06 — DBT Transformations
DBT project connected to Snowflake. Models span three layers:
- [`bronze/`](dbt_stocks/models/bronze/bronze_stg_stock_quotes.sql) — raw structured data
- [`silver/`](dbt_stocks/models/silver/silver_clean_stock_quotes.sql) — cleaned, validated data
- [`gold/`](dbt_stocks/models/gold) — analytical views (Candlestick, KPI, Tree Map)

---

### 07 — Power BI Dashboard
Power BI connected to Snowflake's Gold layer via **Direct Query**. Includes:
- Candlestick chart — stock market patterns
- Tree chart — stock price trends
- Gauge charts — volume & total sales breakdown
- KPI cards — real-time sortable view

---

## Final Deliverables

| Deliverable | Description |
|---|---|
| Automated pipeline | Real-time data flow from API to dashboard |
| Snowflake tables | Bronze, Silver, and Gold layers |
| DBT models | Transformed and validated analytics models |
| Airflow DAGs | Fully orchestrated, scheduled workflows |
| Power BI dashboard | Live insights connected to Gold layer |

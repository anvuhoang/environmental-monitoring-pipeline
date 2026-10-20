# 🌍 ClimateScope — Climate Data Engineering Platform

**An end-to-end data engineering platform for collecting, processing, and analyzing global climate and environmental data.**

ClimateScope is a data engineering project designed to build a unified platform for climate and environmental data from multiple scientific sources, including NASA, NOAA, and other trusted data providers.

The platform uses Apache Airflow to orchestrate data pipelines, Python to ingest and transform datasets, PostgreSQL for structured analytical storage, and Parquet for efficient analytical data processing.

Rather than focusing on a single weather API, ClimateScope aims to integrate multiple climate domains — from global temperature anomalies and greenhouse gas concentrations to sea-level changes, vegetation, sea ice, and extreme weather events.

> **Project status:** 🚧 In Development  
> This project is being developed incrementally, starting with global temperature data and expanding toward a multi-source climate data platform.

---

## 📌 Project Objectives

Climate data is distributed across multiple providers, formats, spatial resolutions, and update schedules. ClimateScope aims to address these challenges by building a reproducible and extensible data engineering pipeline.

Key objectives:

- **Multi-source ingestion:** Collect data from APIs, CSV/TXT files, and scientific datasets.
- **Workflow orchestration:** Use Apache Airflow to schedule, monitor, and manage data pipelines.
- **Data quality:** Implement validation, consistency checks, duplicate detection, and missing-value handling.
- **Data lake foundations:** Preserve original source data in a raw layer for reproducibility and reprocessing.
- **Data transformation:** Convert heterogeneous source data into structured analytical datasets.
- **Analytical storage:** Use PostgreSQL and Parquet for different analytical workloads.
- **Climate analytics:** Enable historical trend analysis and cross-domain exploration.
- **Extensibility:** Support the addition of new datasets without redesigning the entire platform.

## 🌡️ Data Domains

ClimateScope is designed to cover multiple dimensions of climate change and environmental monitoring.

| Domain | Example data | Potential sources |
|---|---|---|
| Atmosphere | Global temperature anomalies, precipitation, wind, humidity | NASA, NOAA |
| Ocean | Sea-surface temperature, sea level, ocean conditions | NASA, NOAA |
| Cryosphere | Sea ice, snow cover, glacier indicators | NASA, NSIDC |
| Biosphere | Vegetation indices, land-cover change | NASA MODIS |
| Extreme events | Wildfires, droughts, heatwaves, extreme rainfall | NASA FIRMS, NOAA |
| Greenhouse gases | Atmospheric CO₂, CH₄, N₂O | NOAA GML |
| Human drivers | Energy consumption, carbon emissions | Global Carbon Project, Our World in Data |

*These are planned data domains, not a claim that all datasets have already been integrated.*

## 🏗️ Architecture

The target architecture separates ingestion, storage, validation, transformation, and analytics.

```text
                 DATA SOURCES
       ┌───────────┼────────────┐
       │           │            │
      NASA        NOAA       Other Sources
       │           │            │
       └───────────┼────────────┘
                   ▼
          INGESTION LAYER
          ┌────────┴────────┐
          │                 │
      File-based         API-based
      Ingestion          Ingestion
          │                 │
          └────────┬────────┘
                   ▼
             RAW DATA LAYER
          Original source data
                   │
                   ▼
            DATA VALIDATION
                   │
                   ▼
           TRANSFORMATION
                   │
                   ▼
           PROCESSED DATA
          ┌────────┴────────┐
          │                 │
      PostgreSQL          Parquet
          │                 │
          └────────┬────────┘
                   ▼
           ANALYTICS LAYER
                   │
                   ▼
          Dashboards & Insights
```

Apache Airflow orchestrates the pipeline stages, manages task dependencies, and supports scheduled execution and failure recovery.

### Data pipeline lifecycle

1. **Extract:** Retrieve data from APIs or download source files.
2. **Store raw data:** Preserve the original response or file.
3. **Validate:** Check schema, data types, missing values, ranges, and duplicates.
4. **Transform:** Normalize dates, column names, units, and source-specific formats.
5. **Load:** Store structured records in PostgreSQL and analytical datasets in Parquet.
6. **Analyze:** Build SQL queries and analytical datasets for climate trend analysis.

## 🛠️ Technology Stack

| Category | Technology | Purpose |
|---|---|---|
| Programming | Python | Ingestion, transformation, validation |
| Orchestration | Apache Airflow | Scheduling and pipeline management |
| Containerization | Docker, Docker Compose | Reproducible environment |
| Database | PostgreSQL | Structured analytical storage |
| File format | CSV, JSON, TXT, Parquet | Source and processed datasets |
| Data processing | Pandas | Tabular data processing |
| Data access | HTTP APIs, HTTP downloads | Source ingestion |
| Testing | Pytest | Unit and pipeline testing |
| Version control | Git, GitHub | Source control and documentation |
| Visualization | Metabase or Grafana | Planned analytics dashboards |
| Object storage | MinIO or S3-compatible storage | Planned scalable raw data storage |

The stack will be introduced incrementally. Additional technologies will be added only when they solve a concrete problem.

## 📂 Project Structure

The target repository structure is organized by responsibility.

```text
ClimateScope/
├── dags/
│   ├── ingestion/
│   ├── quality/
│   ├── transformation/
│   └── analytics/
│
├── src/
│   ├── ingestion/
│   ├── extraction/
│   ├── validation/
│   ├── transformation/
│   ├── database/
│   └── analytics/
│
├── config/
│   └── data_sources.yaml
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── sample/
│
├── sql/
│   ├── schema/
│   ├── staging/
│   └── analytics/
│
├── tests/
│
├── docker/
├── docker-compose.yml
├── requirements.txt
├── .env.example
├── .gitignore
└── README.md
```

*This is the planned structure and may evolve during implementation.*

Design principles:

- Keep DAG files focused on orchestration.
- Put reusable business and data-processing logic in `src/`.
- Keep raw source data separate from transformed data.
- Store SQL scripts independently from Python code.
- Keep credentials and environment-specific settings out of version control.
- Test transformations and data-quality rules independently of Airflow.

## 🚀 Implementation Roadmap

### Phase 1 — Global Temperature Pipeline

**Initial dataset:** NASA GISTEMP v4.

- [ ] Inspect the dataset schema and metadata.
- [ ] Download the global monthly temperature anomaly dataset.
- [ ] Preserve raw data in the local raw storage layer.
- [ ] Build a reusable file-ingestion module.
- [ ] Parse and validate the source data.
- [ ] Transform records into a consistent analytical schema.
- [ ] Export the processed dataset to Parquet.
- [ ] Load observations into PostgreSQL.
- [ ] Orchestrate the pipeline with Airflow.
- [ ] Add automated tests and document the pipeline.

Source: [NASA GISS — GISTEMP](https://data.giss.nasa.gov/gistemp/)

### Phase 2 — Weather and Precipitation

- [ ] Integrate a precipitation or weather observation dataset.
- [ ] Implement API-based ingestion.
- [ ] Handle pagination, rate limits, retries, and API failures where applicable.
- [ ] Standardize timestamps, units, and location identifiers.
- [ ] Add data-quality checks for weather observations.

Potential source: [NOAA National Centers for Environmental Information](https://www.ncei.noaa.gov/)

### Phase 3 — Ocean and Sea-Level Data

- [ ] Integrate sea-level or ocean-temperature datasets.
- [ ] Preserve source metadata and measurement units.
- [ ] Handle time-series observations and location-based measurements.
- [ ] Build analytical datasets for historical trend analysis.

Potential sources: [NASA Sea Level Change](https://sealevel.nasa.gov/) and [NOAA](https://www.noaa.gov/).

### Phase 4 — Cryosphere and Vegetation

- [ ] Integrate sea-ice or snow-cover observations.
- [ ] Explore satellite-derived vegetation indices such as NDVI and EVI.
- [ ] Evaluate the size and spatial structure of scientific datasets.
- [ ] Introduce scalable object storage and partitioned Parquet datasets where needed.

### Phase 5 — Extreme Events and Greenhouse Gases

- [ ] Integrate NASA FIRMS active-fire observations.
- [ ] Add atmospheric greenhouse gas concentration datasets.
- [ ] Implement source-specific validation and monitoring.
- [ ] Analyze historical changes in fire activity and greenhouse gas concentrations.

Potential sources:

- [NASA FIRMS](https://firms.modaps.eosdis.nasa.gov/)
- [NOAA Global Monitoring Laboratory](https://gml.noaa.gov/)

### Phase 6 — Climate Analytics and Dashboard

- [ ] Build SQL-based analytical datasets.
- [ ] Analyze long-term temperature and greenhouse gas trends.
- [ ] Compare climate indicators across time periods.
- [ ] Develop a dashboard for exploring selected indicators.
- [ ] Add pipeline freshness and data-quality monitoring.
- [ ] Document analytical limitations and avoid interpreting correlation as causation.

## 🧪 Data Quality Strategy

ClimateScope will implement explicit quality checks rather than assuming that downloaded data is ready for analysis.

| Check | Example |
|---|---|
| Schema validation | Required columns exist |
| Type validation | Dates and measurements have expected types |
| Completeness | Required observation dates are present |
| Range validation | Latitude is between −90 and 90 |
| Duplicate detection | No duplicate records at the dataset's defined grain |
| Unit consistency | Measurements use documented units |
| Freshness | Latest available data is within the expected update window |
| Reprocessing | Previously ingested data can be safely processed again |

Quality rules will depend on the dataset. Missing observations will not automatically be treated as errors because scientific datasets may legitimately contain gaps.

## 📊 Planned Analytical Use Cases

Once the relevant datasets are integrated, ClimateScope aims to support:

- Historical global temperature anomaly trends.
- Changes in precipitation and weather observations.
- Sea-level trends over time.
- Changes in sea-ice extent and vegetation indices.
- Wildfire activity over time and across geographic regions.
- Atmospheric CO₂ concentration trends.
- Comparisons between climate indicators from different sources.
- Data freshness and pipeline reliability monitoring.

The platform is intended for data exploration and engineering practice. Results will retain source attribution, units, time coverage, and known limitations.

## ⚙️ Getting Started

### Prerequisites

- Python 3.10 or a compatible version for the selected Airflow release.
- Docker and Docker Compose.
- Git.
- Access to the selected public data sources.

The exact Python and Airflow versions should match the project's Docker configuration.

### 1. Clone the repository

```bash
git clone <YOUR_REPOSITORY_URL>
cd ClimateScope
```

Replace `<YOUR_REPOSITORY_URL>` with the actual GitHub repository URL.

### 2. Configure environment variables

```bash
cp .env.example .env
```

On Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

Set the required configuration values for the local environment. Do not commit `.env` or credentials to Git.

### 3. Start the development environment

If Docker Compose has been configured for the project:

```bash
docker compose up -d
```

### 4. Verify the services

```bash
docker compose ps
```

The detailed setup instructions will be expanded as the Airflow, database, and ingestion components are implemented.

## 🔐 Data and Configuration Management

- Public source data should retain its source URL and retrieval metadata.
- Raw data should be preserved separately from transformed outputs.
- Large downloaded datasets should generally be excluded from Git.
- Credentials and API keys should be supplied through environment variables or an appropriate secrets mechanism.
- Source datasets should be re-ingestable without requiring manual changes to the raw files.

## 📚 References

- [NASA GISS Surface Temperature Analysis](https://data.giss.nasa.gov/gistemp/)
- [NOAA National Centers for Environmental Information](https://www.ncei.noaa.gov/)
- [NASA Sea Level Change](https://sealevel.nasa.gov/)
- [NASA FIRMS](https://firms.modaps.eosdis.nasa.gov/)
- [NOAA Global Monitoring Laboratory](https://gml.noaa.gov/)
- [Apache Airflow Documentation](https://airflow.apache.org/docs/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Apache Parquet Documentation](https://parquet.apache.org/docs/)

## 🎯 Project Vision

ClimateScope aims to demonstrate how modern data engineering practices can be applied to scientific and environmental datasets.

The long-term goal is to create a reproducible, extensible climate data platform that combines heterogeneous sources into a consistent foundation for historical analysis and environmental insight.

**Built with Python, Apache Airflow, PostgreSQL, Docker, and a focus on reliable data pipelines.**

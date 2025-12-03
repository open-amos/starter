**AMOS** | [Core](https://github.com/open-amos/core) | [Reconciliation](https://github.com/open-amos/reconciliation) | [Dashboard](https://github.com/open-amos/dashboard) | [Source Example](https://github.com/open-amos/source-example)

---

# AMOS

**The open-source data infrastructure for private market funds.**

AMOS replaces spreadsheet chaos with a modern data stack. It connects your existing systems into a unified SQL warehouse, giving you a reliable foundation to automate reporting and deploy AI without vendor lock-in.

![image](https://img.shields.io/badge/version-0.1.0-blue?style=for-the-badge) ![image](https://img.shields.io/badge/status-public--beta-yellow?style=for-the-badge)
![image](https://img.shields.io/badge/dbt-FF694B?style=for-the-badge&logo=dbt&logoColor=white) ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) ![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E) ![TypeScript](https://img.shields.io/badge/typescript-%23007ACC.svg?style=for-the-badge&logo=typescript&logoColor=white) ![Svelte](https://img.shields.io/badge/svelte-%23f1413d.svg?style=for-the-badge&logo=svelte&logoColor=white)

---

## Who It’s For

  - **Emerging funds** looking for a lightweight but robust data foundation to support efficient operations and AI-readiness from day one.
  - **Established funds** seeking to modernize legacy systems and simplify complex data architectures without vendor lock-in.

## Quick Start

The fastest way to spin up the full stack is via Docker.

```bash
# 1. Clone the repo
git clone [https://github.com/open-amos/amos.git](https://github.com/open-amos/amos.git)
cd amos

# 2. Launch the stack
docker-compose up -d

# 3. Access the UI
# Dashboard: http://localhost:3000
# Database: localhost:5432
```

If you prefer to run this in your own environment without Docker:

1.  Install [dbt-core and dbt-postgres](https://docs.getdbt.com/docs/get-started-dbt) or any other adapter. AMOS works with any database engine supported by dbt, including PostgreSQL, MySQL, Microsoft SQL Server, Snowflake, Google BigQuery, Amazon Redshift, Databricks, and more.
2.  Configure your database connections in `profiles.yml`
3.  Run `dbt deps` to install dependencies (amos-core and amos-source-example)
4.  Run `dbt seed` to load example data
5.  Run `dbt run` to build the warehouse
6.  Run `dbt test` to validate metrics

## Project Components

This repository (`amos`) acts as the orchestrator. The logic is modularized across the following repos:

| Repository | Purpose | Who is this for? |
| :--- | :--- | :--- |
| **[AMOS Core](https://github.com/open-amos/core)** | **The Brain.** The dbt models and SQL logic that power the platform. | Data Engineers extending the logic. |
| **[AMOS Reconciliation](https://github.com/open-amos/reconciliation)** <br/>*(Coming Soon)* | **The Glue.** A UI for mapping entities (e.g., "Seq" = "Sequoia") and resolving data conflicts across systems. | Data Stewards & Ops Teams. |
| **[AMOS Dashboard](https://github.com/open-amos/dashboard)** | **The UI.** A reference implementation of a BI dashboard for private markets. | Frontend devs & UI customizers. |
| **[AMOS Source Example](https://github.com/open-amos/source-example)** | **The Template.** Example data loaders and transformation patterns. | Engineers building new integrations. |

## System Architecture

```mermaid
  graph TD
      S1[Excel/CSV Files]
      S2[CRM Systems]
      S3[Fund Administrator]
      S4[Portfolio Management]
      S5[Accounting Systems]
      S6[More Sources...]
      
      ING[<b>Ingestion & Model Mapping</b><br/><i>AMOS Source Example</i>]
      CORE[<b>Canonical Models & Metrics</b><br/><i>AMOS Core</i>]
      
      DASH[<b>BI Dashboards</b><br/><br/><i>AMOS Dashboard</i>, Power BI, Tableau, Metabase...]
      APP[<b>Custom Applications<br/></b><br/><i>AMOS API</i><br/><i>coming soon</i>]
      AI[<b>LLM integration</b><br/><br/><i>AMOS MCP</i><br/><i>-coming soon-</i>]
      
      S1 --> ING
      S2 --> ING
      S3 --> ING
      S4 --> ING
      S5 --> ING
      S6 --> ING
      
      ING --> CORE
      
      CORE --> DASH
      CORE --> APP
      CORE -->|Semantic Layer| AI
      
      style ING fill:#E8E4F3
      style CORE fill:#D6E9F8
      style DASH fill:#FFF4E6
      style APP fill:#E8F5E9
      style AI fill:#E8F5E9
```

## Demo

Explore a live dashboard built on top of the AMOS architecture.
[**→ View live demo**](https://demo.amos.tech)
![image](https://amos.tech/wp-content/uploads/2025/04/screely-1764599384361-2048x1175.png)

## Current Scope

AMOS currently supports core private market workflows:

  - **Canonical Models:** Private Equity & Private Credit.
  - **Integration Patterns:** Deal Pipeline, Portfolio Management, Fund Admin, and Accounting.

## Roadmap

AMOS is evolving toward broader coverage. Upcoming milestones:

  - 🤝 **Visual Reconciliation:** A dedicated app to map entities and resolve data conflicts across systems without writing SQL.
  - 🤖 **AI-Assisted Mapping:** "Human-in-the-loop" suggestions for mapping messy entity names (e.g., matching "Google Inc" to "Alphabet").
  - 🟢 **Impact & ESG:** Standardized models for impact measurement.
  - 🔌 **Rest API:** Public API for programmatic access to metrics.
  - 🤖 **MCP Server:** Native integration for AI assistants (Claude/ChatGPT) to query your data.
  - 📊 **Utility Apps:** Tools for automated PDF data extraction.

## Extensibility

AMOS is built to be extended:

  * **Connect real data:** Swap the `Source Example` for your own custom dbt models connecting to raw data.
  * **Extend the logic:** Import `amos-core` as a package and build your own marts on top of it.

## Licensing

AMOS is currently in **Public Beta**. Each subproject README describes temporary licensing terms. Final open-source or source-available licenses will be finalized before the 1.0 release.

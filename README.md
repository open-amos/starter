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
7.  Install [AMOS Dashboard](https://github.com/open-amos/dashboard) manually

## Project Components

This repository (`amos`) acts as the orchestrator. The logic is modularized across the following repos:

| Repository | Purpose | Who is this for? |
| :--- | :--- | :--- |
| **[AMOS Core](https://github.com/open-amos/core)** | **The Brain.** The dbt models and SQL logic that power the platform. | Data Engineers extending the logic. |
| **[AMOS Reconciliation](https://github.com/open-amos/reconciliation)** <br/>*(Coming Soon)* | **The Glue.** A UI for mapping entities (e.g., `crm.company_id` = `portfolio.company_id`) and resolving data conflicts across systems. | Data Stewards & Ops Teams. |
| **[AMOS Dashboard](https://github.com/open-amos/dashboard)** | **The UI.** A reference implementation of a BI dashboard for private markets. | Frontend devs & UI customizers. |
| **[AMOS Source Example](https://github.com/open-amos/source-example)** | **The Template.** Example data loaders and transformation patterns. | Engineers building new integrations. |

## System Architecture

```mermaid
graph TD
    %% --- STYLES ---
    classDef sourceNode fill:#f9f9f9,stroke:#999,stroke-width:1px,stroke-dasharray: 5 5;
    classDef transportNode fill:#fff3e0,stroke:#f57c00,stroke-width:2px;
    classDef infraNode fill:#eceff1,stroke:#546e7a,stroke-width:2px;
    classDef engineNode fill:#e3f2fd,stroke:#1565c0,stroke-width:2px;
    classDef semanticNode fill:#e1f5fe,stroke:#0277bd,stroke-width:2px;
    classDef outputNode fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
    
    %% Style for the AMOS Product Boundary
    classDef amosContainer fill:#f5faff,stroke:#1565c0,stroke-width:2px,stroke-dasharray: 5 5;
    %% Style for the Core Repo Boundary (The Brain)
    classDef coreContainer fill:#ffffff,stroke:#0277bd,stroke-width:2px;
    %% Style for the User's Warehouse Boundary
    classDef warehouseContainer fill:#fafafa,stroke:#90a4ae,stroke-width:2px;

    %% --- 1. EXTERNAL SYSTEMS ---
    subgraph External_Source_Systems [1. External Source Systems]
        direction LR
        S1[📄 Excel/CSV Files]
        S2[🤝 CRM Systems]
        S3[🏦 Fund Admin]
        S4[🏢 PortCo Data]
        S5[📒 Accounting]
    end

    %% --- TRANSPORT LAYER ---
    TRANSPORT[<b>🚚 Data Transport</b><br/><i>Fivetran, Airbyte, Uploads</i>]

    %% --- THE WAREHOUSE ---
    subgraph User_Warehouse_Infra [2. Your Data Warehouse Infrastructure <br>SQL Server / Postgres / Snowflake / BigQuery]
        direction TB

        %% Landing Zone
        RAW[<b>🛢️ Raw Database / Landing Zone</b><br/><i>Data lands here as-is.<br>AMOS Source Example provides example seed data.</i>]

        %% --- THE AMOS PRODUCT ---
        subgraph AMOS_Stack [✨ The AMOS Stack]
            direction TB

            %% Ingestion & Prep
            subgraph Prep_Layer [3. Ingest & Repair]
                direction TB
                ING[<b>🔀 Clean & Map</b><br/><i>AMOS Sources</i>]
                RECON[<b>🧩 Reconcile Entities</b><br/><i>AMOS Reconciliation</i>]
            end

            %% AMOS CORE (The "Brain" Wrapper)
            subgraph AMOS_Core_Repo [4. AMOS Core 🧠]
                direction TB
                MODEL[<b>💎 Canonical Data Models</b><br/><i>Standardized Tables </i>]
                METRICS[<b>📐 Semantic Layer</b><br/><i>Metric Definitions & Joins</i>]
                
                MODEL --> METRICS
            end
            
            %% Consumption Layer
            subgraph Consumption_Layer [5. Consumption]
                direction TB
                DASH[<b>📈 BI Dashboards</b><br/><i>AMOS Dashboard</i><br>Power BI, Tableau...]
                APP[<b>⚡ Custom Apps</b><br/><i>AMOS API</i>]
                AI[<b>🤖 AI Agents</b><br/><i>AMOS MCP</i>]
            end
        end
    end

    %% --- CONNECTIONS ---
    S1 & S2 & S3 & S4 & S5 --> TRANSPORT
    TRANSPORT ==> RAW
    
    %% Flow into AMOS
    RAW --> ING
    ING --> RECON
    RECON --> MODEL
    
    %% Flow to Consumption
    METRICS --> DASH
    METRICS --> APP
    METRICS --> AI

    %% --- CLASS ASSIGNMENT ---
    class S1,S2,S3,S4,S5 sourceNode;
    class TRANSPORT transportNode;
    class RAW infraNode;
    class ING,RECON,MODEL engineNode;
    class METRICS semanticNode;
    class DASH,APP,AI outputNode;
    
    class AMOS_Stack amosContainer;
    class User_Warehouse_Infra warehouseContainer;
    class AMOS_Core_Repo coreContainer;
```
## How It Works

AMOS acts as the open-source operating system that runs on top of your existing data warehouse. It connects your fragmented data exports and transforms them into a single, unified source of truth.

  - 🚚 Transport (You bring the data): Use your existing tools (Fivetran, Airbyte) or simple file uploads to drop raw data into your warehouse's "Landing Zone."
  - 🧩 Ingest & Repair: AMOS picks up that raw data. It standardizes column names and uses a reconciliation layer to resolve entity conflicts (e.g., mapping "Seq Capital" in your CRM to "Sequoia" in your Fund Admin system)
  - 💎 The Core Brain: The data is transformed into a Canonical Data Model—a rigorous, single source of truth. At this stage, standard financial metrics (IRR, TVPI, Exposure) are calculated once, centrally.
  - 🚀 Consume: Your BI dashboards, custom apps, and AI agents query these clean, pre-calculated metrics. They never touch the messy raw data.

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

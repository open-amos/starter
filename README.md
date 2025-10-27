# AMOS Starter

**Open-source, production-ready data warehouse for private equity, venture capital, and alternative investment funds.**

The easiest way to get started with AMOS—just configure your data warehouse connection and run.

## Why AMOS Starter

Building a PE/VC data warehouse from scratch is complex. AMOS Starter provides a complete, open-source starting point that you can deploy quickly and extend over time.

## What is AMOS Starter?

AMOS Starter is the orchestrator that ties together the AMOS packages to deliver an end-to-end pipeline—from source systems to analytics-ready data.

**What you get:**
- Multi-source integration patterns (CRM, fund admin, portfolio mgmt, accounting)
- Canonical dimensional model (AMOS Core)
- Entity resolution framework
- Example data to run immediately
- Cloud-agnostic design (Snowflake, BigQuery, Databricks, Redshift, etc.)

## The AMOS Ecosystem

| Package | Purpose | Layer | Status |
|---------|---------|-------|--------|
| **AMOS Starter** (this package) | Orchestrator | Orchestration | ⭐ Start here |
| **AMOS Source Example** | Source integration | Staging + Intermediate | Imported |
| **AMOS Core** | Canonical model | Marts | Imported |

**You only work with AMOS Starter**—the other packages are automatically imported and coordinated.

## Architecture

Four-layer warehouse: RAW → STAGING → INTERMEDIATE → CORE → Analytics

## Quick Start

### Install & Run

```bash
dbt deps
dbt seed --select amos_source_example   # Load example data from example package
dbt run                                  # Build all models from imported packages
dbt test                                 # Run tests
```

See the documentation for environment setup, profiles, and advanced selectors.

## What You Get

- Prebuilt dimensions, facts, and bridges (via AMOS Core)
- Source integration patterns and example data (via AMOS Source Example)
- Repeatable build, test, and documentation workflows

## Customization

1. **Connect real data sources** - Replace seeds with your own systems
2. **Adapt staging models** - Match your source schemas
3. **Configure entity resolution** - Populate cross-reference tables
4. **Extend the model** - Add custom dimensions or facts
5. **Adjust database names** - Update environment variables

## What's Coming

AMOS is actively developed with upcoming features:
- **Pre-built metrics** - Comprehensive, BI-ready metrics for common analyses
- **Reconciliation UI** - Human-in-the-loop entity resolution interface
- **REST API** - Documented API for programmatic access
- **Report generator** - Word/PDF report builder with templates

## Documentation

For complete documentation, visit **[docs.amos.tech](https://docs.amos.tech)**

## Project Structure

```
amos_starter/              # This orchestrator package
├── dbt_project.yml      # Profile and configurations
├── packages.yml         # Package dependencies
└── models/              # (empty - models in imported packages)

amos_source_example/      # Source integration (imported)
├── seeds/               # Example source data
└── models/              # Staging + intermediate

amos_core/               # Canonical model (imported)
├── seeds/              # Reference data
└── models/marts/       # Dimensions + facts
```

## Troubleshooting

See the docs for common setup issues and selector tips.

## Contributing

AMOS is open source and welcomes contributions. Report bugs, suggest features, add integration patterns, or submit pull requests.

## Support

- **Documentation**: [docs.amos.tech](https://docs.amos.tech)
- **Issues**: GitHub Issues
- **Community**: Join discussions and share best practices

## Related Projects

- **[AMOS Core](../amos_core/README.md)** - Canonical dimensional model
- **[AMOS Source Example](../amos_source_example/README.md)** - Source integration patterns

# AMOS

[Overview](https://github.com/open-amos/) | **Starter** | [Core](https://github.com/open-amos/core) | [Source Example](https://github.com/open-amos/source-example) | [Dashboard](https://github.com/open-amos/dashboard-example) 

---

# AMOS Starter

AMOS Starter is a thin dbt project that coordinates AMOS Core and AMOS Source Example to deliver an end-to-end pipeline—from source systems to analytics-ready data.

![image](https://img.shields.io/badge/version-0.1.0-blue?style=for-the-badge) ![image](https://img.shields.io/badge/status-proof--of--concept-yellow?style=for-the-badge) ![image](https://img.shields.io/badge/dbt-FF694B?style=for-the-badge&logo=dbt&logoColor=white)

## Quick Start

### Install & Run

1. Install [dbt](https://docs.getdbt.com/docs/get-started-dbt) 
2. Configure your database connections in your dbt `profiles.yml` file
3. Git clone this repository
4. Run `dbt deps` to install the dependencies
5. Run `dbt seed` to load the example data
6. Run `dbt run` to build the models
7. Run `dbt test` to run the tests

## Customization

1. **Connect real data sources** - Replace seeds with your own systems
2. **Adapt staging models** - Match your source schemas
3. **Extend AMOS Core** - Add custom dimensions, facts, or metrics (in separate packages)

## Contributing

AMOS is open source and welcomes contributions. Report bugs, suggest features, add integration patterns, or submit pull requests.

## Support

- **Documentation**: [docs.amos.tech](https://docs.amos.tech)
- **Issues**: GitHub Issues

## Related Projects

- **[AMOS Core](../core)** - Canonical dimensional model
- **[AMOS Source Example](../source-example)** - Source integration patterns
- **[AMOS Dashboard](../dashboard-example)** - Example analytics and KPI dashboards built on AMOS Core

## Licensing

This subproject is part of the AMOS public preview. Licensing terms will be finalized before version 1.0.
For now, the code is shared for evaluation and feedback only. Commercial or production use requires written permission from the maintainers.

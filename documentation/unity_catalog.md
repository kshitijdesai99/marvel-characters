# Catalog

- Schema / Database
  - Table / View / Model / Function / Volume

`Catalog.schema.table` → `production.sales.orders`

Unity Catalog provides centralized access control, lineage, auditing, and discovery across Databricks workspaces.

A catalog does not necessarily mean the physical data is inside Databricks. It is more like a registered namespace + governance layer.

The actual data may live in:

- S3
- ADLS
- GCS
- Delta Lake
- Iceberg
- External databases

But Databricks uses the catalog to know:

- What the object is
- Where it lives
- Who can access it
- How it is organized

## Installation

```bash
brew tap databricks/tap
brew install databricks
databricks auth login --host https://dbc-75ab9b7a-9f88.cloud.databricks.com
uv sync --extra dev
source .venv/bin/activate
```

## Setup Notes

- Updated `databricks.yml` file
- Go to **Workspace → Users** and create a Git folder

## Pydantic

- Type hints are for humans & the IDE
- Pydantic helps avoid checking `isinstance` for every argument one by one

**Typical flow:**

`YAML file` → `Python dictionary` → `Pydantic object` → `Pass object into training / preprocessing / MLflow functions`
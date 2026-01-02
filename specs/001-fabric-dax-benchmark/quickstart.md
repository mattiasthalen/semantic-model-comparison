# Quickstart: Fabric DAX Benchmarking Report

**Date**: 2025-12-31

## Goal

Run the marimo benchmark report locally to execute a batch and review narrative
results.

## Prerequisites

- uv installed for dependency management
- Access to Microsoft Fabric semantic model datasets
- DefaultAzureCredential configured (e.g., Azure CLI login)
- DAX query files under the `dax/` directory

## Setup

1. Create a Python environment and install dependencies using uv:
   - `uv venv`
   - `uv pip install marimo pandas numpy plotly httpx uuid6 azure-identity python-dotenv respx pytest`
2. Provide secrets via environment variables (optionally with a local `.env`).
3. Provide non-secret defaults via marimo inputs or environment variables.
4. Place DAX query files under `dax/` using deterministic folder structure.

## Required Environment Variables (Secrets)

- `POWERBI_TENANT_ID`
- `POWERBI_CLIENT_ID`
- `POWERBI_CLIENT_SECRET`
- `KQL_TENANT_ID`
- `KQL_CLIENT_ID`
- `KQL_CLIENT_SECRET`

## Required Environment Variables (Non-Secrets)

- `FABRIC_WORKSPACE_NAME` (default: `TPC-DS`)
- `FABRIC_DATASET_NAMES` (default: `Star Schema,Unified Star Schema`)
- `FABRIC_EVENTHOUSE_NAME` (default: `Monitoring Eventhouse`)
- `FABRIC_EVENTSTREAM_NAME` (default: `Monitoring_Eventstream`)
- `FABRIC_KQL_DATABASE_NAME` (default: `Monitoring KQL database`)

## Run the Report

1. Launch the marimo app:
   - `marimo run benchmark_report.py`
2. Confirm the workspace name and dataset list (defaults to TPC-DS with Star Schema
   and Unified Star Schema).
3. Review narrative sections without execution to confirm intent and methodology.
4. Execute the benchmark batch to produce telemetry-driven results.

## Verify

- The report shows runs for each dataset and DAX query.
- Visualizations render comparisons by dataset, dax_group, run_type, and dax_name.
- Failed runs are clearly labeled with counts.
- `dax/single_fact/top_n_sales.dax` is included in the executed suite.

## Offline Test Notes

- Tests use static HTTP fixtures so runs are offline and deterministic.
- Tests run from silent notebook cells to satisfy TDD without separate test files.

## Troubleshooting

- Missing DAX files: verify `dax/` folder structure and file names.
- Telemetry not found: re-run telemetry retrieval and confirm batch/run IDs.
- Partial failures: review run status table to identify failed runs.

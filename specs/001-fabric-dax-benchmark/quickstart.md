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

1. Create a Python environment and install dependencies using uv.
2. Provide secrets via environment variables (optionally with a local `.env`).
3. Provide non-secret defaults via marimo inputs or hard-coded values as needed.
4. Place DAX query files under `dax/` using deterministic folder structure.

## Required Environment Variables (Secrets)

- `AZURE_TENANT_ID`
- `AZURE_CLIENT_ID`
- `AZURE_CLIENT_SECRET`

## Run the Report

1. Launch the marimo app:
   - `app/benchmark_report.py`
2. Enter workspace name, dataset names, and Eventhouse name when prompted.
3. Review narrative sections without execution to confirm intent and methodology.
4. Execute the benchmark batch to produce telemetry-driven results.

## Verify

- The report shows runs for each dataset and DAX query.
- Visualizations render comparisons by dataset, dax_group, run_type, and dax_name.
- Failed runs are clearly labeled with counts.

## Offline Test Notes

- Tests use static HTTP fixtures so runs are offline and deterministic.

## Troubleshooting

- Missing DAX files: verify `dax/` folder structure and file names.
- Telemetry not found: re-run telemetry retrieval and confirm batch/run IDs.
- Partial failures: review run status table to identify failed runs.

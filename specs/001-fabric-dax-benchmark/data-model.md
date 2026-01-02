# Data Model: Fabric DAX Benchmarking Report

**Date**: 2025-12-31

## Entities

### Batch

- **Fields**:
  - batch_id (UUIDv7, required, unique)
  - created_at (timestamp, required)
  - workspace_name (string, required, default TPC-DS)
  - datasets (list of DatasetRef, required, min 2)
  - dax_suite (list of DaxQueryRef, required, non-empty)
  - run_type (enum: cold | warm, required)
  - status (enum: pending | running | completed | completed_with_failures | failed)
- **Validation rules**:
  - batch_id must be UUIDv7
  - datasets must be >= 2
  - run_type must be applied consistently across all datasets in the batch

### Run

- **Fields**:
  - run_id (UUIDv7, required, unique)
  - batch_id (UUIDv7, required, FK -> Batch.batch_id)
  - dataset_id (string, required, semantic model ID)
  - dataset_display_name (string, required)
  - run_type (enum: cold | warm, required)
  - dax_name (string, required)
  - dax_group (string, required)
  - started_at (timestamp, optional)
  - completed_at (timestamp, optional)
  - status (enum: pending | running | succeeded | failed)
  - execution_status (string, optional)
- **Validation rules**:
  - run_id must be UUIDv7
  - run_type must match the batch run_type
  - dax_name derived from dax file name (no extension)
  - dax_group derived from dax/ folder hierarchy

### DatasetRef

- **Fields**:
  - dataset_display_name (string, required)
  - dataset_id (string, required once resolved, semantic model ID)
- **Validation rules**:
  - dataset_display_name must be non-empty

### DaxQueryRef

- **Fields**:
  - dax_path (string, required)
  - dax_name (string, required)
  - dax_group (string, required)
- **Validation rules**:
  - dax_path must be under dax/
  - dax_name and dax_group derived deterministically from dax_path

### TelemetryRecord

- **Fields**:
  - run_id (UUIDv7, required)
  - batch_id (UUIDv7, required)
  - dataset_id (string, required, semantic model ID)
  - dataset_display_name (string, optional)
  - dax_name (string, required)
  - dax_group (string, required)
  - run_type (enum: cold | warm, required)
  - timestamp (timestamp, required)
  - duration_ms (number, required)
  - status (enum: available | missing)
- **Validation rules**:
  - run_id and batch_id must correlate to recorded runs
  - duration_ms must be non-negative

### Report

- **Fields**:
  - batch_id (UUIDv7, required)
  - narrative_sections (list of strings, required)
  - figures (list of FigureRef, required)
  - tables (list of TableRef, required)
  - summary_metrics (dictionary, required)
  - failed_run_summary (counts and identifiers)
- **Validation rules**:
  - narrative_sections must include intent, methodology, results, conclusions
  - figures/tables must include captions

### FigureRef / TableRef

- **Fields**:
  - title (string, required)
  - caption (string, required)
  - grouping (enum: dataset | dax_group_dataset | run_type_dataset | dax_name_dataset)
- **Validation rules**:
  - grouping must match one of the required report comparisons

## Relationships

- Batch 1..* Run
- Batch 1..* TelemetryRecord (via run correlation)
- Batch 1..1 Report
- Run 0..1 TelemetryRecord (record may be missing if logs delayed)

## State Transitions

- Batch: pending -> running -> completed | completed_with_failures | failed
- Run: pending -> running -> succeeded | failed

## Notes

- Telemetry retrieval may require retries but must eventually correlate every recorded
  run by batch_id and run_id before analysis.
- Dataset and Eventhouse identifiers are resolved by name at runtime.

# Data Model

## Batch

- batch_id (UUIDv7, required)
- run_type (enum: cold|warm, required)
- started_at (timestamp)
- completed_at (timestamp)
- datasets (list of Dataset)
- dax_queries (list of DaxQuery)
- runs (list of Run)

## Run

- run_id (UUIDv7, required)
- batch_id (UUIDv7, required)
- ds (semantic model ID, required)
- dataset_name (display name, required)
- run_type (enum: cold|warm, required)
- dax_name (string, required)
- dax_group (string, required)
- status (enum: success|failed, required)
- http_status (int, optional)
- error (string, optional)
- started_at (timestamp)
- completed_at (timestamp)

## Dataset

- ds (semantic model ID, required)
- dataset_name (display name, required)
- workspace_id (string, required)

## DaxQuery

- dax_path (string, required)
- dax_name (string, required)
- dax_group (string, required)
- dax_text (string, required)

## TelemetryRecord

- batch_id (UUIDv7, required)
- run_id (UUIDv7, required)
- ds (semantic model ID, required)
- dataset_name (display name, required)
- dax_name (string, required)
- dax_group (string, required)
- run_type (enum: cold|warm, required)
- started_at (timestamp, required)
- completed_at (timestamp, required)
- duration_ms (number, required)
- missing (bool, required)
- raw_fields (dict, optional)

## ReportSection

- section_id (string, required)
- title (string, required)
- narrative (string, required)
- figures (list, optional)
- tables (list, optional)

# Research: Fabric DAX Benchmarking Report

**Date**: 2025-12-31  
**Context**: Resolve technical choices for executing DAX via REST and retrieving
telemetry via KQL in a notebook-first delivery.

## Decision 1: DAX Execution API

- **Decision**: Use Power BI REST API `executeQueries` in group for DAX execution.
- **Rationale**: The API is the supported REST interface for running DAX queries
  against a dataset in a workspace, with explicit limits and error behavior.
- **Alternatives considered**: Sempy evaluate_dax, XMLA, CLI tools.
- **References**:
  - https://learn.microsoft.com/en-us/rest/api/power-bi/datasets/execute-queries-in-group

## Decision 2: DAX API Limits & Throttling

- **Decision**: Run sequentially and throttle to stay under 120 requests/min per user.
- **Rationale**: The executeQueries API is limited to 120 requests/min and one query
  per call; sequential execution avoids concurrency issues.
- **Alternatives considered**: Parallel calls with backoff.
- **References**:
  - https://learn.microsoft.com/en-us/rest/api/power-bi/datasets/execute-queries-in-group

## Decision 3: KQL Telemetry Query Interface

- **Decision**: Use KQL REST API `/v1/rest/query` against the Eventhouse query service.
- **Rationale**: KQL REST is the documented HTTPS query interface for Fabric KQL
  databases.
- **Alternatives considered**: Kqlmagic, Spark connectors.
- **References**:
  - https://learn.microsoft.com/en-us/kusto/api/rest

## Decision 4: Resolve Eventhouse Query Service URI

- **Decision**: Call Fabric Eventhouse Get API to retrieve `queryServiceUri`.
- **Rationale**: The Eventhouse GET response provides the `queryServiceUri` needed to
  send KQL REST queries.
- **Alternatives considered**: Hard-coded query URI.
- **References**:
  - https://learn.microsoft.com/en-us/rest/api/fabric/eventhouse/items/get-eventhouse

## Decision 5: Authentication Strategy

- **Decision**: Use DefaultAzureCredential and separate auth flows for REST and KQL.
- **Rationale**: Aligns with project constraints and allows distinct scopes for Power
  BI REST and KQL REST access.
- **Alternatives considered**: Client secret only, shared token for both services.

## Decision 6: Dependency Management

- **Decision**: Use uv for dependency management.
- **Rationale**: Fast, reproducible installs and alignment with CLI workflows.
- **Alternatives considered**: pip + venv, poetry.

## Decision 7: Configuration Strategy

- **Decision**: Use environment variables with optional `.env` for secrets; marimo
  inputs for non-secret defaults.
- **Rationale**: Keeps secrets out of source while allowing interactive setup.
- **Alternatives considered**: config files only, environment-only.

## Decision 8: Testing Strategy

- **Decision**: pytest with respx/httpx fixtures; tests live in silent notebook cells.
- **Rationale**: TDD compliance with deterministic, offline tests.
- **Alternatives considered**: vcrpy, pytest-httpx.

## Decision 9: Dataset Resolution

- **Decision**: Resolve datasets by name at runtime and cache IDs for the full batch.
- **Rationale**: Avoids hard-coded IDs while keeping runs stable within a batch.
- **Alternatives considered**: manual ID entry.

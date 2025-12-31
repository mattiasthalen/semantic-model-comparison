# Research: Fabric DAX Benchmarking Report

**Date**: 2025-12-31
**Context**: Resolve technical context choices for the marimo-based benchmark report.

## Decision 1: Language/Version

- **Decision**: Python 3.11
- **Rationale**: marimo is Python-native; Python provides strong data tooling for
  telemetry analysis and visualization.
- **Alternatives considered**: Python 3.10, Python 3.12

## Decision 2: Dependency Management

- **Decision**: uv
- **Rationale**: Fast, modern Python dependency management aligned with CLI workflows.
- **Alternatives considered**: pip + venv, poetry

## Decision 3: Primary Dependencies

- **Decision**: marimo, pandas, numpy, plotly, httpx, uuid6, azure-identity,
  python-dotenv, respx
- **Rationale**: marimo is the narrative app; pandas/numpy aggregate telemetry; plotly
  provides portable visuals; httpx is preferred for REST calls; uuid6 provides UUIDv7;
  azure-identity supplies DefaultAzureCredential; python-dotenv supports local .env;
  respx mocks httpx for offline tests.
- **Alternatives considered**: matplotlib, seaborn, requests, uuid7

## Decision 4: Authentication

- **Decision**: DefaultAzureCredential (azure-identity)
- **Rationale**: Standard Azure auth flow for local/dev and managed identity contexts.
- **Alternatives considered**: client secret only, managed identity only

## Decision 5: Configuration Strategy

- **Decision**: Secrets from environment variables (optionally .env); non-secrets from
  marimo inputs with defaults or hard-coded values.
- **Rationale**: Keeps secrets out of source while maintaining convenience for local
  use.
- **Alternatives considered**: all config in env vars, config files only

## Decision 6: Testing and Mocking

- **Decision**: pytest with respx and static fixtures for external calls
- **Rationale**: TDD is required; respx supports httpx mocking; static fixtures allow
  offline tests without CI record mode.
- **Alternatives considered**: unittest, pytest-httpx, vcrpy

## Decision 7: Target Platform

- **Decision**: Desktop (Linux/macOS/Windows)
- **Rationale**: marimo runs locally and is suitable for a portable, readable report.
- **Alternatives considered**: hosted web app

## Decision 8: Performance Goals

- **Decision**: Generate report outputs for up to 100 runs within 5 minutes
- **Rationale**: Keeps feedback loops fast while accommodating typical batch sizes.
- **Alternatives considered**: no explicit goal, 10-minute target

## Decision 9: Scale/Scope Assumptions

- **Decision**: 2-10 datasets, 10-100 DAX queries, hundreds of runs per batch
- **Rationale**: Aligns with expected benchmarking sessions while remaining practical
  for local execution.
- **Alternatives considered**: single dataset, thousands of queries

## Decision 10: Name-Based Resolution

- **Decision**: Resolve workspace, dataset, Eventhouse, and KQL context by name using
  Fabric list endpoints at runtime.
- **Rationale**: Avoids hard-coded IDs and keeps inputs human-friendly.
- **Alternatives considered**: manual ID entry, static configuration files

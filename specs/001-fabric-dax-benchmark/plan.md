# Implementation Plan: Fabric DAX Benchmarking Report

**Branch**: `001-fabric-dax-benchmark` | **Date**: 2025-12-31 | **Spec**: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`
**Input**: Feature specification from `/specs/001-fabric-dax-benchmark/spec.md`

## Summary

Deliver a single marimo notebook (`.py`) that reads as a narrative report while running
sequential DAX benchmarks via the Fabric REST API against the TPC-DS workspace datasets
Star Schema and Unified Star Schema (supporting additional datasets later). The
notebook loads DAX files from `dax/`, derives `dax_name` and `dax_group`, runs each DAX
query per dataset with UUIDv7 `batch_id` and `run_id` tagging, and then queries
telemetry via KQL against Eventstream Monitoring_Eventstream / Eventhouse Monitoring
Eventhouse / KQL database Monitoring KQL database. Only narrative/report cells render
visible output; implementation and tests live in silent cells. Dependencies are managed
with uv and HTTP calls use httpx with offline-testable mocks.

## Technical Context

**Language/Version**: Python 3.11  
**Primary Dependencies**: marimo, pandas, numpy, plotly, httpx, uuid6, azure-identity,
python-dotenv, respx, pytest  
**Storage**: N/A (local `dax/` files only)  
**Testing**: pytest (offline; respx/httpx mocks, deterministic fixtures)  
**Target Platform**: Local desktop (Linux/macOS/Windows)  
**Project Type**: Single marimo notebook (`.py`) at repo root  
**Performance Goals**: Complete up to ~100 sequential runs in under 5 minutes; throttle
to remain below Power BI REST API limits (120 requests/min per user).  
**Constraints**: Single notebook file (no `src/`), REST + KQL use separate auth,
workspace name fixed to `TPC-DS`, datasets resolved by name and cached per batch,
env-var config with optional `.env`, offline tests only.  
**Scale/Scope**: 2 datasets initially; 1-100 DAX files; sequential execution only.

## Constitution Check

- TDD plan: tests defined in silent notebook cells before implementation changes; core
  functions are pure and unit-testable with mocked HTTP.
- Small, single-purpose functions with explicit data flow (e.g., DAX loading, dataset
  resolution, REST execution, telemetry query, analysis).
- UX consistency: narrative/report cells show status, progress, and errors explicitly;
  implementation cells are silent.
- Performance work: rate-limit awareness and telemetry-driven measurements documented.

## Project Structure

### Documentation (this feature)

```text
/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── data-model.md        # Phase 1 output (/speckit.plan command)
├── quickstart.md        # Phase 1 output (/speckit.plan command)
├── contracts/           # Phase 1 output (/speckit.plan command)
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)

```text
/home/mattiasthalen/repos/semantic-model-comparison/
├── benchmark_report.py
└── dax/
    └── single_fact/
        └── top_n_sales.dax
```

**Structure Decision**: Single marimo notebook (`benchmark_report.py`) is the only
primary code artifact. All implementation logic and tests live in silent notebook
cells; narrative/report cells render the report output.

## Phase 0: Outline & Research

- Confirm Fabric REST API usage for DAX execution, including rate limits and required
  scopes.
- Confirm KQL REST API endpoint format for Eventhouse queries.
- Confirm Eventhouse lookup to obtain `queryServiceUri` for KQL query execution.
- Consolidate decisions in `research.md`.

**References**:
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/research.md`

## Phase 1: Design & Contracts

- Update `data-model.md` for batch/run IDs, dataset resolution + caching, DAX
  identifiers, and telemetry correlation fields.
- Update `contracts/benchmark-api.yaml` to document outbound REST calls:
  Power BI executeQueries and KQL REST query endpoints.
- Update `quickstart.md` to reflect uv usage, env vars, and notebook execution path.
- Run `/home/mattiasthalen/repos/semantic-model-comparison/.specify/scripts/bash/update-agent-context.sh codex`.

**References**:
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/contracts/benchmark-api.yaml`
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/quickstart.md`

## Phase 2: Implementation Plan

1. Create `benchmark_report.py` marimo notebook skeleton with narrative sections and
   silent implementation/test cells. References: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`
2. Implement config loader (env vars + optional `.env`) with defaults for workspace and
   dataset names (TPC-DS, Star Schema, Unified Star Schema). References:
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/quickstart.md`,
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`
3. Implement DAX loader to enumerate `dax/` files, derive `dax_name` and `dax_group`,
   and ensure `dax/single_fact/top_n_sales.dax` is included. References:
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`
4. Implement dataset resolution by name in workspace; cache semantic model IDs and
   display names for the full batch. References:
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`,
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`
5. Implement DAX execution client using Fabric REST API executeQueries; generate
   UUIDv7 `batch_id` and `run_id` and tag all runs with required metadata. References:
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/research.md`,
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/contracts/benchmark-api.yaml`,
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`
6. Implement telemetry retrieval via KQL REST query against Monitoring Eventhouse /
   Monitoring KQL database; correlate by `batch_id` and `run_id` with retries.
   References:
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/research.md`,
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/contracts/benchmark-api.yaml`,
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`
7. Implement analysis and report visuals (median metrics; groupings by dataset,
   dax_group, run_type, dax_name) with explicit failed-run reporting. References:
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`,
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`
8. Implement deterministic tests in silent cells using respx/httpx fixtures for REST
   and KQL calls; confirm offline execution. References:
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/research.md`,
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/quickstart.md`

**References**:
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/research.md`
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/contracts/benchmark-api.yaml`
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/quickstart.md`

## Complexity Tracking

No constitution violations identified.

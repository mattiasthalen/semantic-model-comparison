# Implementation Plan: Fabric DAX Benchmarking Report

**Branch**: `001-fabric-dax-benchmark` | **Date**: 2025-12-31 | **Spec**: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`
**Input**: Feature specification from `/specs/001-fabric-dax-benchmark/spec.md`

## Summary

Deliver a single marimo notebook that reads as a narrative report without execution, runs a shared DAX suite sequentially across multiple Fabric semantic model datasets, and correlates telemetry by batch_id/run_id to produce median-based comparisons. The plan is MVP-first and strongly iterative, with each vertical slice producing a runnable notebook state and an in-notebook green pytest run before moving to the next slice. Endpoint/KQL details are intentionally left as small TODO contracts when unknown.

## Technical Context

**Language/Version**: Python 3.11
**Primary Dependencies**: marimo, pandas, numpy, plotly, httpx, uuid6, azure-identity, pytest
**Storage**: Local files under `/home/mattiasthalen/repos/semantic-model-comparison/dax/`
**Testing**: pytest executed against the notebook via marimo pytest-in-notebook (`uv run pytest <notebook.py>`)
**Target Platform**: Local execution on Linux/macOS/Windows with network access to Fabric
**Project Type**: Single notebook report file (no `src/` package by default)
**Performance Goals**: Accurate, reproducible median runtime analysis; sequential execution with clear run ordering
**Constraints**: Offline CI tests using mocks/recorded fixtures; implementation/test cells must be silent; uv-only dependency management
**Scale/Scope**: 2+ datasets, tens of DAX files, sequential execution, report-first UX

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

- Confirm TDD plan (tests written first, unit-testable core, deterministic tests). **PASS**: Each slice starts with notebook test cells, using mocks/fixtures for external calls.
- Confirm design favors small, single-purpose functions with explicit data flow. **PASS**: Core logic isolated into small helpers in non-test cells.
- Confirm UX consistency expectations (naming, structure, explicit status/errors). **PASS**: Narrative sections are explicit; execution status/errors are recorded and surfaced in report tables.
- Confirm performance work is data-driven with measurement plan if needed. **PASS**: Medians computed from telemetry; no premature optimization.

## Project Structure

### Documentation (this feature)

```text
/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/
├── plan.md
├── research.md
├── data-model.md
├── quickstart.md
├── contracts/
│   ├── benchmark-api.yaml (legacy reference; verify alignment or deprecate)
│   ├── fabric-exec.todo.md
│   └── fabric-telemetry.todo.md
└── tasks.md
```

### Source Code (repository root)

```text
/home/mattiasthalen/repos/semantic-model-comparison/
├── fabric_dax_benchmark.py
└── dax/
    └── single_fact/
        └── top_n_sales.dax
```

**Structure Decision**: Single marimo notebook at repository root to satisfy the single-artifact requirement, with DAX files under the existing `dax/` directory.

## Iterative MVP-First Plan

Each slice ends with a runnable notebook state and a green in-notebook pytest run via `uv run pytest /home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`.

### Slice 1: Notebook boot + narrative skeleton (no execution)

- Create marimo notebook with narrative/report cells only producing output.
- Include intent, methodology, results (placeholder), and conclusions sections readable without execution.
- Add implementation cells marked silent; no external calls.

### Slice 2: Auth/config (tests first)

- Tests: env var loading and validation for execution auth and telemetry auth (separate flows). Use mock env values.
- Implement config loader from env vars; include a minimal connectivity check stub with TODO endpoint placeholder.
- Keep stub silent and non-executed by default.

### Slice 3: GUID lookup for datasets (tests first)

- Tests: mocked REST responses resolve workspace "TPC-DS" and datasets "Star Schema" and "Unified Star Schema" by name, cache IDs per batch.
- Implement name resolution helper with TODO endpoint contracts.

### Slice 4: DAX loading (tests first)

- Tests: load DAX files from `dax/` and deterministically derive `dax_name` and `dax_group` from path.
- Implement file discovery and parsing helpers.

### Slice 5: Single DAX execution (tests first)

- Tests: UUIDv7 batch_id per session and UUIDv7 run_id per execution; ensure tags include batch_id, run_id, ds, run_type, dax_name, dax_group.
- Implement sequential execution stub via REST with success determined by HTTP status only; TODO endpoint contract.

### Slice 6: Expand to all datasets + all DAX files (tests first)

- Tests: run_type tagging (cold/warm), continue-on-error behavior, complete coverage over datasets and queries.
- Implement loop over datasets and DAX suite; record failures and continue.

### Slice 7: Telemetry retrieval + correlation (tests first)

- Tests: mocked/recorded responses for telemetry retrieval; correlation by batch_id/run_id with missing markers.
- Implement telemetry query using TODO KQL contract against Eventstream/Eventhouse/KQL DB; keep auth separate.

### Slice 8: Analysis + plots (tests first)

- Tests: aggregation logic for medians and groupings (dataset, dax_group x dataset, run_type x dataset, dax_name x dataset).
- Implement pandas aggregation and plotly figures; narrative cells render figures/tables.

## MVP Deliverable

A single notebook that renders a narrative report without execution, can execute a batch sequentially with mocked tests offline, correlates telemetry by batch_id/run_id, and produces median-based comparisons, while keeping implementation/test cells silent.

## Non-Goals

- Validating DAX query correctness
- Persisting results to long-term storage
- Automatic tuning or declaring a winner

## Open TODO Contracts (Minimal)

- TODO(Fabric Exec Endpoint): exact REST path, headers, and response schema for DAX execution
- TODO(Fabric Dataset Lookup): exact REST path for workspace/dataset name resolution
- TODO(Fabric Telemetry KQL): final KQL schema and query text for runtime metrics

## Complexity Tracking

None.

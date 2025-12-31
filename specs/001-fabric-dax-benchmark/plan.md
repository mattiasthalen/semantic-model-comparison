# Implementation Plan: Fabric DAX Benchmarking Report

**Branch**: `001-fabric-dax-benchmark` | **Date**: 2025-12-31 | **Spec**: /home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md
**Input**: Feature specification from `/specs/001-fabric-dax-benchmark/spec.md`

**Note**: This template is filled in by the `/speckit.plan` command. See `.specify/templates/commands/plan.md` for the execution workflow.

## Summary

Implement a Python-based marimo report that benchmarks multiple Microsoft Fabric
semantic model datasets with a shared DAX suite, correlates runtime telemetry by
batch/run IDs, and presents narrative-first comparisons without declaring a winner.
The design uses TDD with pytest, keeps core logic in small, mostly pure functions with
thin I/O boundaries, resolves Fabric items by name via list endpoints at runtime, and
runs tests offline with static fixtures.

## Technical Context

**Language/Version**: Python 3.11
**Primary Dependencies**: marimo, pandas, numpy, plotly, httpx, uuid6, azure-identity,
python-dotenv, respx
**Storage**: Local files for ephemeral run/telemetry cache; no long-term persistence
**Testing**: pytest (TDD), respx for httpx mocking, static fixtures for offline tests
**Target Platform**: Desktop (Linux/macOS/Windows)
**Project Type**: single
**Performance Goals**: Generate report outputs for up to 100 runs within 5 minutes
**Constraints**: Use Microsoft Fabric REST API; use DefaultAzureCredential for auth;
secrets only from environment variables or .env; non-secret config may be hard-coded or
marimo defaults; resolve workspace/dataset/eventhouse IDs by name via list endpoints;
no query result inspection; no long-term storage; prefer httpx unless strong
justification; dependency management via uv
**Scale/Scope**: 2-10 datasets, 10-100 DAX queries, hundreds of runs per batch

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

- Confirm TDD plan (tests written first, unit-testable core, deterministic tests).
- Confirm design favors small, single-purpose functions with explicit data flow.
- Confirm UX consistency expectations (naming, structure, explicit status/errors).
- Confirm performance work is data-driven with measurement plan if needed.

Status: PASS (no violations identified).
Post-Design Check: PASS (design artifacts align with core principles).

## Project Structure

### Documentation (this feature)

```text
specs/001-fabric-dax-benchmark/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── data-model.md        # Phase 1 output (/speckit.plan command)
├── quickstart.md        # Phase 1 output (/speckit.plan command)
├── contracts/           # Phase 1 output (/speckit.plan command)
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)

```text
app/
└── benchmark_report.py  # marimo entrypoint

src/
├── benchmark/
│   ├── __init__.py
│   ├── config.py
│   ├── dax_loader.py
│   ├── runner.py
│   ├── telemetry.py
│   ├── analysis.py
│   └── report_state.py
├── clients/
│   ├── __init__.py
│   ├── fabric_api.py
│   └── kql_api.py
└── report/
    ├── __init__.py
    ├── narrative.py
    └── visuals.py

tests/
├── unit/
├── integration/
├── contract/
└── fixtures/
```

**Structure Decision**: Single-project layout with marimo in `app/` and pure logic in
`src/` to keep execution isolated, testable, and aligned with functional style.

## Core Implementation Sequence

1. **Inputs & config**: Capture workspace name, dataset names, Eventhouse name, and
   run_type in marimo; load secrets from env only. See `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/quickstart.md`
   and `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`.
2. **Name resolution**: Resolve workspace/dataset/Eventhouse/KQL context by name using
   list endpoints; store resolved IDs in config state. See
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/contracts/benchmark-api.yaml`
   and `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`.
3. **DAX discovery**: Load DAX files from `dax/`, derive dax_name/dax_group from the
   folder structure. See `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`
   and `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`.
4. **Batch + run creation**: Create a batch_id and sequential run_id entries per
   dataset × query; attach required metadata. See
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`
   and `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`.
5. **Execution**: Execute runs sequentially via Fabric execution interface; record
   success status only. See `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`.
6. **Telemetry retrieval**: After runs complete, retrieve telemetry with retries and
   correlate by batch_id/run_id; mark missing telemetry explicitly. See
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`
   and `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`.
7. **Analysis**: Compute median runtime metrics and prepare grouped aggregates. See
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`.
8. **Visualization + narrative**: Render required comparison views and narrative
   sections; clearly label failures and avoid winner language. See
   `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`.

## Refinement & Hardening Sequence

1. **Test-first harness**: Unit tests for DAX loading, name resolution mapping,
   run scheduling, telemetry correlation, and aggregation. Use static fixtures for
   Fabric/KQL calls. See `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/plan.md`
   (Testing) and `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/quickstart.md`.
2. **Edge cases**: Missing DAX files, delayed/missing telemetry, partial failures,
   dataset unavailability. See `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`.
3. **UX consistency**: Ensure explicit status/progress/error indicators in the report.
   See `/home/mattiasthalen/repos/semantic-model-comparison/.specify/memory/constitution.md`
   and `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`.
## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| N/A | N/A | N/A |

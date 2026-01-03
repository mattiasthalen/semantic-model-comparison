---

description: "Task list for Fabric DAX Benchmarking Report"
---

# Tasks: Fabric DAX Benchmarking Report

**Input**: Design documents from `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: REQUIRED. Use marimo pytest-in-notebook workflow with `uv run pytest /home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`.

**Organization**: Tasks grouped by user story with strict TDD per vertical slice: add failing test-only cells first, implement minimal code to pass, then run tests / ensure green before moving on.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and notebook scaffolding

- [ ] T001 [P] Initialize uv project dependencies in `/home/mattiasthalen/repos/semantic-model-comparison/pyproject.toml` and `/home/mattiasthalen/repos/semantic-model-comparison/uv.lock`
- [ ] T002 [P] Create marimo notebook shell with app header and section markers in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core structures shared across all user stories

- [ ] T003 Add failing test-only cell for core entity structures (Batch/Run/Dataset/DaxQuery/TelemetryRecord) in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T004 Implement minimal dataclass structures and shared constants (TODO endpoint/KQL placeholders) in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T005 Run tests and ensure green for foundational structures via `uv run pytest /home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`

**Checkpoint**: Foundation ready - user story implementation can now begin

---

## Phase 3: User Story 1 - Read the benchmark report (Priority: P1) 🎯 MVP

**Goal**: Narrative report is readable without execution and contains required sections.

**Independent Test**: Open the notebook without execution and confirm intent, methodology, results placeholders, and conclusions with captions.

### Tests for User Story 1 (REQUIRED)

- [ ] T006 [US1] Add failing test-only cells asserting narrative section content/ordering and that only narrative cells render output in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`

### Implementation for User Story 1

- [ ] T007 [US1] Implement narrative content helpers/constants and non-output implementation cells in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T008 [US1] Add narrative/report cells rendering intent, methodology, results placeholders, and conclusions with captions in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T009 [US1] Run tests and ensure green for US1 via `uv run pytest /home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`

**Checkpoint**: User Story 1 is fully functional and independently testable

---

## Phase 4: User Story 2 - Run a benchmark batch (Priority: P2)

**Goal**: Execute a benchmark batch sequentially across datasets with required metadata.

**Independent Test**: Execute a mocked batch and confirm one run per dataset per DAX query with required tags.

### Slice 2: Auth/config (tests first)

- [ ] T010 [US2] Add failing test-only cells for env-based execution/telemetry auth config and connectivity check stub in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T011 [US2] Implement config loader and connectivity check stub using TODO endpoint contract in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T012 [US2] Run tests and ensure green for auth/config via `uv run pytest /home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`

### Slice 3: GUID lookup for datasets (tests first)

- [ ] T013 [US2] Add failing test-only cells for workspace/dataset name resolution with mocked REST responses and per-batch caching in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T014 [US2] Implement dataset resolver with TODO endpoint contract and cache in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T015 [US2] Run tests and ensure green for dataset lookup via `uv run pytest /home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`

### Slice 4: DAX loading (tests first)

- [ ] T016 [US2] Add failing test-only cells for DAX file loading and deterministic dax_name/dax_group derivation from `/home/mattiasthalen/repos/semantic-model-comparison/dax/` in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T017 [US2] Implement DAX discovery and parsing helpers in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T018 [US2] Run tests and ensure green for DAX loading via `uv run pytest /home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`

### Slice 5: Single DAX execution (tests first)

- [ ] T019 [US2] Add failing test-only cells for UUIDv7 batch/run IDs and tag propagation (batch_id, run_id, ds, run_type, dax_name, dax_group) in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T020 [US2] Implement single-run execution stub using TODO Fabric exec contract (success by HTTP status only) in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T021 [US2] Run tests and ensure green for single execution via `uv run pytest /home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`

### Slice 6: Expand to all datasets + all DAX files (tests first)

- [ ] T022 [US2] Add failing test-only cells for run_type tagging (cold/warm), sequential ordering, and continue-on-error behavior in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T023 [US2] Implement full batch execution loop over datasets and DAX suite with failure recording in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T024 [US2] Run tests and ensure green for batch execution via `uv run pytest /home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`

**Checkpoint**: User Story 2 is fully functional and independently testable

---

## Phase 5: User Story 3 - Interpret behavioral comparisons (Priority: P3)

**Goal**: Retrieve telemetry, compute medians, and render comparative plots without declaring a winner.

**Independent Test**: Run mocked telemetry retrieval and aggregation to produce grouped median tables/plots and narrative conclusions.

### Slice 7: Telemetry retrieval + correlation (tests first)

- [ ] T025 [US3] Add failing test-only cells for telemetry retrieval, correlation by batch_id/run_id, and missing markers in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T026 [US3] Implement telemetry client stub using TODO KQL contract and correlation logic in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T027 [US3] Run tests and ensure green for telemetry retrieval via `uv run pytest /home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`

### Slice 8: Analysis + plots (tests first)

- [ ] T028 [US3] Add failing test-only cells for median aggregation and required groupings (dataset, dax_group x dataset, run_type x dataset, dax_name x dataset) in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T029 [US3] Implement aggregation helpers and plotly figures; render results in narrative cells in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T030 [US3] Run tests and ensure green for analysis/plots via `uv run pytest /home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`

**Checkpoint**: User Story 3 is fully functional and independently testable

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Final consistency, clarity, and verification

- [ ] T031 Ensure conclusions emphasize behavioral differences without ranking and summarize failures/missing telemetry in `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T032 Run full notebook test suite and confirm green via `uv run pytest /home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`
- [ ] T033 Validate quickstart commands and adjust if needed in `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/quickstart.md`

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - blocks all user stories
- **User Stories (Phase 3+)**: Depend on Foundational phase completion
- **Polish (Phase 6)**: Depends on all desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2)
- **User Story 2 (P2)**: Depends on User Story 1 completion for narrative baseline and notebook structure
- **User Story 3 (P3)**: Depends on User Story 2 for run metadata and run records

### Within Each Slice

- Test-only cells first (must fail)
- Minimal implementation to pass tests
- Run tests / ensure green before proceeding

---

## Parallel Execution Examples

### User Story 1

No safe parallel tasks; all changes occur within `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py`.

### User Story 2

No safe parallel tasks; all changes occur within `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py` and must follow strict slice order.

### User Story 3

No safe parallel tasks; all changes occur within `/home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py` and must follow strict slice order.

---

## Implementation Strategy

### MVP First

1. Complete Setup (Phase 1)
2. Complete Foundational (Phase 2)
3. Complete User Story 1 (Phase 3) with strict TDD and green tests
4. Stop and validate the narrative report without execution

### Incremental Delivery

1. Add User Story 2 slices (auth/config → lookup → DAX loading → single run → batch)
2. Add User Story 3 slices (telemetry → analysis/plots)
3. Final polish and full test run

### TDD Enforcement

- Each slice adds failing test-only cells first
- Minimal implementation to pass
- Explicit test run checkpoint before moving on

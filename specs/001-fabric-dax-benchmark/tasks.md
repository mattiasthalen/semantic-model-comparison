---

description: "Task list for Fabric DAX Benchmarking Report"
---

# Tasks: Fabric DAX Benchmarking Report

**Input**: Design documents from `/specs/001-fabric-dax-benchmark/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: Required by default (TDD); all tests should be written to fail before implementation.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- [ ] T001 Create `pyproject.toml` with Python 3.11, uv deps, pytest, ruff, and project metadata
- [ ] T002 Create package and test skeletons in `app/benchmark_report.py`, `src/benchmark/__init__.py`, `src/clients/__init__.py`, `src/report/__init__.py`, `tests/unit/`, `tests/integration/`, `tests/contract/`, `tests/fixtures/`
- [ ] T003 [P] Add environment template in `.env.example` with required secret keys

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [ ] T004 Define core dataclasses and validation helpers for Batch, Run, DatasetRef, DaxQueryRef, TelemetryRecord in `src/benchmark/report_state.py`
- [ ] T005 [P] Implement config loading for env and marimo inputs in `src/benchmark/config.py`
- [ ] T006 [P] Implement UUIDv7 helpers in `src/benchmark/ids.py`
- [ ] T007 [P] Add shared HTTP client setup and auth plumbing in `src/clients/fabric_api.py`
- [ ] T008 [P] Add shared HTTP client setup and auth plumbing in `src/clients/kql_api.py`
- [ ] T009 [P] Write unit tests for config and env handling in `tests/unit/test_config.py`
- [ ] T010 [P] Write unit tests for core dataclasses and validation in `tests/unit/test_report_state.py`
- [ ] T011 [P] Write unit tests for UUIDv7 generation in `tests/unit/test_ids.py`

**Checkpoint**: Foundation ready - user story implementation can now begin

---

## Phase 3: User Story 1 - Read the benchmark report (Priority: P1) 🎯 MVP

**Goal**: Deliver a narrative-first marimo report that is readable without execution.

**Independent Test**: Open the notebook without execution and verify narrative sections explain intent, methodology, datasets compared, and conclusions with supporting captions.

### Tests for User Story 1 (REQUIRED)

- [ ] T012 [P] [US1] Write unit test for required narrative sections in `tests/unit/test_narrative.py`
- [ ] T013 [P] [US1] Write unit test for non-executed figure/table placeholders and captions in `tests/unit/test_report_layout.py`

### Implementation for User Story 1

- [ ] T014 [P] [US1] Implement narrative section builders in `src/report/narrative.py`
- [ ] T015 [P] [US1] Implement non-executed visuals placeholders and captions in `src/report/visuals.py`
- [ ] T016 [US1] Compose the static report view in `app/benchmark_report.py` using narrative + placeholders

**Checkpoint**: User Story 1 is readable without execution and independently testable

---

## Phase 4: User Story 2 - Run a benchmark batch (Priority: P2)

**Goal**: Execute sequential DAX runs across datasets with consistent metadata and telemetry correlation.

**Independent Test**: Execute a batch and confirm each dataset has one run per DAX query with required metadata, plus telemetry correlation or explicit missing markers.

### Tests for User Story 2 (REQUIRED)

- [ ] T017 [P] [US2] Add offline fixtures for Fabric list and batch APIs in `tests/fixtures/fabric/`
- [ ] T018 [P] [US2] Add offline fixtures for KQL telemetry in `tests/fixtures/kql/`
- [ ] T019 [P] [US2] Write contract tests for resolution endpoints in `tests/contract/test_fabric_resolution_contracts.py`
- [ ] T020 [P] [US2] Write contract tests for batch and telemetry endpoints in `tests/contract/test_fabric_batch_contracts.py`
- [ ] T021 [P] [US2] Write unit tests for DAX discovery rules in `tests/unit/test_dax_loader.py`
- [ ] T022 [P] [US2] Write unit tests for run scheduling metadata in `tests/unit/test_runner_schedule.py`
- [ ] T023 [P] [US2] Write unit tests for telemetry correlation and missing markers in `tests/unit/test_telemetry.py`

### Implementation for User Story 2

- [ ] T024 [P] [US2] Implement DAX discovery and grouping in `src/benchmark/dax_loader.py`
- [ ] T025 [P] [US2] Implement name resolution and batch endpoints in `src/clients/fabric_api.py`
- [ ] T026 [P] [US2] Implement KQL context lookup and telemetry retrieval in `src/clients/kql_api.py`
- [ ] T027 [P] [US2] Implement run scheduling and sequential execution in `src/benchmark/runner.py`
- [ ] T028 [P] [US2] Implement telemetry retrieval, retry, and correlation in `src/benchmark/telemetry.py`
- [ ] T029 [US2] Wire execution flow and local cache updates in `app/benchmark_report.py`

**Checkpoint**: User Story 2 runs a full batch with correlated telemetry and explicit failures

---

## Phase 5: User Story 3 - Interpret behavioral comparisons (Priority: P3)

**Goal**: Provide analysis and summaries that emphasize behavioral differences without declaring a winner.

**Independent Test**: Review summaries and visuals to confirm they avoid winner language and show the four required comparisons.

### Tests for User Story 3 (REQUIRED)

- [ ] T030 [P] [US3] Write unit tests for median aggregation and grouping in `tests/unit/test_analysis.py`
- [ ] T031 [P] [US3] Write unit tests for summary language constraints in `tests/unit/test_summary_language.py`

### Implementation for User Story 3

- [ ] T032 [P] [US3] Implement aggregation helpers in `src/benchmark/analysis.py`
- [ ] T033 [P] [US3] Implement comparison visuals in `src/report/visuals.py`
- [ ] T034 [US3] Implement behavioral summaries in `src/report/narrative.py` and integrate in `app/benchmark_report.py`

**Checkpoint**: User Story 3 comparisons and summaries are independently verifiable

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Improvements that affect multiple user stories

- [ ] T035 [P] Update quickstart validation steps in `specs/001-fabric-dax-benchmark/quickstart.md`
- [ ] T036 [P] Add performance notes and runtime expectations in `specs/001-fabric-dax-benchmark/plan.md`
- [ ] T037 Run quickstart.md validation checklist in `specs/001-fabric-dax-benchmark/quickstart.md`

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3+)**: All depend on Foundational phase completion
- **Polish (Final Phase)**: Depends on all desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational - no dependencies on other stories
- **User Story 2 (P2)**: Can start after Foundational - uses shared config/clients
- **User Story 3 (P3)**: Can start after Foundational - depends on US2 outputs

---

## Parallel Execution Examples

### User Story 1

```bash
Task: "Write unit test for required narrative sections in tests/unit/test_narrative.py"
Task: "Write unit test for non-executed figure/table placeholders and captions in tests/unit/test_report_layout.py"
```

### User Story 2

```bash
Task: "Write unit tests for DAX discovery rules in tests/unit/test_dax_loader.py"
Task: "Write unit tests for run scheduling metadata in tests/unit/test_runner_schedule.py"
Task: "Write unit tests for telemetry correlation and missing markers in tests/unit/test_telemetry.py"
```

### User Story 3

```bash
Task: "Write unit tests for median aggregation and grouping in tests/unit/test_analysis.py"
Task: "Write unit tests for summary language constraints in tests/unit/test_summary_language.py"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational (blocks all stories)
3. Complete Phase 3: User Story 1
4. Stop and validate User Story 1 independently

### Incremental Delivery

1. Complete Setup + Foundational
2. Add User Story 1 → Validate
3. Add User Story 2 → Validate
4. Add User Story 3 → Validate
5. Polish and cross-cutting tasks


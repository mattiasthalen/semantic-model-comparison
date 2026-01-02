---

description: "Task list for Fabric DAX Benchmarking Report"
---

# Tasks: Fabric DAX Benchmarking Report

**Input**: Design documents from `/specs/001-fabric-dax-benchmark/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: Tests are REQUIRED by the constitution and TDD mandate; write tests first in silent notebook cells.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- [ ] T001 Create the single marimo notebook scaffold with silent/test cells and narrative cells in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py`
- [ ] T002 [P] Add a minimal marimo app entry cell and non-executed narrative skeleton in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py`
- [ ] T003 [P] Verify sample DAX file exists and add fallback content if missing in `/home/mattiasthalen/repos/semantic-model-comparison/dax/single_fact/top_n_sales.dax`
- [ ] T004 [P] Create local HTTP fixture directory for offline tests in `/home/mattiasthalen/repos/semantic-model-comparison/fixtures/` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/research.md`, `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/quickstart.md`)

**References**:
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/quickstart.md`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [ ] T005 Write failing tests for config/env parsing and defaults in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py`
- [ ] T006 Write failing tests for UUIDv7 batch/run ID generation and tagging in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py`
- [ ] T007 Write failing tests for DAX file discovery and `dax_group`/`dax_name` derivation in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`)
- [ ] T008 Write failing tests for dataset name resolution and per-batch cache behavior in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`, `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`)
- [ ] T009 Implement config loader (env + optional `.env`) and defaults in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/quickstart.md`)
- [ ] T010 Implement UUIDv7 helpers and run metadata tagging in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`, `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`)
- [ ] T011 Implement DAX loader and deterministic `dax_group`/`dax_name` logic in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`)
- [ ] T012 Implement dataset resolution by name with cached semantic model IDs in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`, `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`)
- [ ] T013 Implement HTTP client wrappers (httpx + respx hooks) for offline tests in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/research.md`)

**Checkpoint**: Foundation ready - user story implementation can now begin

**References**:
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/research.md`
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/quickstart.md`

---

## Phase 3: User Story 1 - Read the benchmark report (Priority: P1) 🎯 MVP

**Goal**: Narrative report readable without execution, with clear methodology and results framing

**Independent Test**: Open the notebook without execution and verify intent/methodology/results/conclusions sections render with captions

### Tests for User Story 1 (REQUIRED) ⚠️

- [ ] T014 [P] [US1] Write failing tests asserting narrative section text/structure exists in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`)
- [ ] T015 [P] [US1] Write failing tests asserting report figures/tables include captions in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`)

### Implementation for User Story 1

- [ ] T016 [US1] Implement narrative/report cells (intent, methodology, results, conclusions) in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`)
- [ ] T017 [US1] Add placeholder figures/tables with captions for non-executed view in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`)
- [ ] T018 [US1] Add explicit status/progress/error narrative placeholders for UX consistency in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`)

**Checkpoint**: User Story 1 should be fully readable without execution

**References**:
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`

---

## Phase 4: User Story 2 - Run a benchmark batch (Priority: P2)

**Goal**: Execute sequential DAX runs across datasets with full metadata tagging and telemetry retrieval

**Independent Test**: Execute a batch and verify one run per dataset per DAX query with batch/run IDs and required tags

### Tests for User Story 2 (REQUIRED) ⚠️

- [ ] T019 [P] [US2] Write failing tests for Fabric REST executeQueries request/response handling using fixtures in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/research.md`, `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/contracts/benchmark-api.yaml`)
- [ ] T020 [P] [US2] Write failing tests for sequential run execution and tag propagation in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`, `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`)
- [ ] T021 [P] [US2] Write failing tests for KQL telemetry retrieval and correlation by batch/run IDs in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/research.md`, `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/contracts/benchmark-api.yaml`, `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`)

### Implementation for User Story 2

- [ ] T022 [US2] Implement Fabric REST executeQueries client and throttled sequential runner in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/research.md`, `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/contracts/benchmark-api.yaml`)
- [ ] T023 [US2] Implement batch/run execution pipeline with per-run metadata tagging and status tracking in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`, `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`)
- [ ] T024 [US2] Implement Eventhouse lookup + KQL REST query client for telemetry in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/research.md`, `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/contracts/benchmark-api.yaml`)
- [ ] T025 [US2] Implement telemetry correlation and missing-record markers in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`, `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`)
- [ ] T026 [US2] Add offline fixtures for REST/KQL responses in `/home/mattiasthalen/repos/semantic-model-comparison/fixtures/` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/research.md`)

**Checkpoint**: User Story 2 runs end-to-end with offline mocks

**References**:
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/research.md`
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/contracts/benchmark-api.yaml`
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/quickstart.md`

---

## Phase 5: User Story 3 - Interpret behavioral comparisons (Priority: P3)

**Goal**: Provide median-based comparisons and visual summaries without declaring a winner

**Independent Test**: Render summaries/visuals that emphasize behavioral differences without ranking datasets

### Tests for User Story 3 (REQUIRED) ⚠️

- [ ] T027 [P] [US3] Write failing tests for median aggregation and required groupings in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`, `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`)
- [ ] T028 [P] [US3] Write failing tests for failed-run labeling in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`)

### Implementation for User Story 3

- [ ] T029 [US3] Implement telemetry analysis (median metrics, groupings) in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`, `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`)
- [ ] T030 [US3] Implement report visuals for dataset, dax_group×dataset, run_type×dataset, dax_name×dataset in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`)
- [ ] T031 [US3] Implement conclusions text to avoid declaring a winner in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`)
- [ ] T032 [US3] Implement failed run summaries and labels in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`)

**Checkpoint**: User Story 3 visuals and summaries render correctly

**References**:
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/data-model.md`

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Improvements that affect multiple user stories

- [ ] T033 [P] Add edge-case handling (missing DAX files, dataset unavailable, delayed telemetry) in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`)
- [ ] T034 [P] Add explicit progress/error messaging for batch execution in `/home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`)
- [ ] T035 Run quickstart verification checklist and align notes in `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/quickstart.md` (refs: `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/quickstart.md`)

**References**:
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/spec.md`
- `/home/mattiasthalen/repos/semantic-model-comparison/specs/001-fabric-dax-benchmark/quickstart.md`

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3+)**: All depend on Foundational phase completion
- **Polish (Phase 6)**: Depends on desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2)
- **User Story 2 (P2)**: Can start after Foundational (Phase 2)
- **User Story 3 (P3)**: Can start after Foundational (Phase 2)

### Within Each User Story

- Tests MUST be written and FAIL before implementation
- Core helpers before orchestration/visuals
- Story complete before moving to next priority

### Parallel Opportunities

- T002, T003, T004 can run in parallel
- Test tasks within a user story are parallelizable
- Fixture prep (T026) can run in parallel with other US2 implementation tasks

---

## Parallel Example: User Story 2

```bash
Task: "Write failing tests for Fabric REST executeQueries request/response handling using fixtures in /home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py"
Task: "Write failing tests for sequential run execution and tag propagation in /home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py"
Task: "Write failing tests for KQL telemetry retrieval and correlation by batch/run IDs in /home/mattiasthalen/repos/semantic-model-comparison/benchmark_report.py"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational
3. Complete Phase 3: User Story 1
4. **STOP and VALIDATE**: Confirm narrative report is readable without execution

### Incremental Delivery

1. Complete Setup + Foundational → Foundation ready
2. Add User Story 1 → Test independently → MVP
3. Add User Story 2 → Test independently → Batch execution
4. Add User Story 3 → Test independently → Behavioral comparisons

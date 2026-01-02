# Feature Specification: Fabric DAX Benchmarking Report

**Feature Branch**: `001-fabric-dax-benchmark`  
**Created**: 2025-12-31  
**Status**: Draft  
**Input**: User description: "Specify a system that benchmarks two or more Microsoft
Fabric semantic model datasets by running a shared DAX query suite and analyzing
runtime telemetry. The goal is to understand behavioral and usage characteristics, not
to validate results or declare a winner. The primary artifact is a narrative report
that is readable without execution and reproducible when run. Runs are sequential, and
each dataset in a batch uses the same query set and configuration. Telemetry is
correlated to each run and used to generate median-based comparisons across datasets,
query groups, and run types. Failed runs are recorded and clearly marked. Non-goals
include validating DAX results, long-term persistence, automatic tuning, or declaring a
winner."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Read the benchmark report (Priority: P1)

As a reader, I want a clear narrative report of methodology and results without running
anything so I can understand the benchmark intent and conclusions quickly.

**Why this priority**: The report is the primary artifact and must stand alone for
stakeholders who do not execute the notebook.

**Independent Test**: Open the notebook without execution and verify the narrative
explains the methodology, datasets compared, and key conclusions with supporting
figures/tables.

**Acceptance Scenarios**:

1. **Given** the notebook is opened without execution, **When** I read the narrative
   sections, **Then** I can identify intent, approach, and conclusions.
2. **Given** the notebook is opened without execution, **When** I review the results
   section, **Then** I can see figures/tables with captions that explain what is shown.

---

### User Story 2 - Run a benchmark batch (Priority: P2)

As an analyst, I want to execute a benchmark batch across multiple datasets using the
same DAX query set so I can compare runtime telemetry fairly.

**Why this priority**: The system's value depends on running a consistent, comparable
benchmark across datasets.

**Independent Test**: Execute a batch and confirm every selected dataset is evaluated
against the same DAX suite with recorded run metadata.

**Acceptance Scenarios**:

1. **Given** a batch is configured with datasets and a DAX suite, **When** execution
   finishes, **Then** each dataset has one run per DAX query with required metadata.

---

### User Story 3 - Interpret behavioral comparisons (Priority: P3)

As a stakeholder, I want summaries and visual comparisons that highlight behavioral or
usage differences without implying absolute technical superiority.

**Why this priority**: The benchmark is intended for characterization, not ranking or
validation of correctness.

**Independent Test**: Review the report summaries to confirm they emphasize observed
behavioral differences and avoid declaring a winner.

**Acceptance Scenarios**:

1. **Given** the report summaries and visualizations, **When** I read the conclusions,
   **Then** they frame differences as behavioral/usage characteristics rather than
   absolute performance superiority.

### Edge Cases

- What happens when a DAX file is missing or cannot be read?
- How does the system label runs when telemetry logs are delayed or incomplete?
- How are partial batches presented when some runs fail?
- What happens if a dataset becomes unavailable mid-batch?

## Clarifications

### Session 2025-12-31

- Q: What values and semantics define run_type? → A: run_type MUST be one of cold or
  warm, reflecting execution context (e.g., cache state) and applied consistently across
  all datasets within a batch.
- Q: When may telemetry logs be retrieved? → A: Telemetry may be retrieved after runs
  complete and/or with retries, but must correlate every recorded run by batch_id and
  run_id.
- Q: What should ds represent? → A: ds MUST be the semantic model ID, and the dataset
  display name MUST also be recorded separately.
- Q: Should execution and telemetry use separate authentication? → A: Yes. Execution
  access and telemetry query access MUST use separate authentication flows/credentials.
- Q: How should DAX execution be performed? → A: Use a supported Fabric execution
  interface for DAX queries, with implementation details defined in the plan.
- Q: How should required datasets be identified? → A: Resolve datasets by name at
  runtime and cache the resolved IDs for the entire run.
- Q: Should implementation live in a single notebook file? → A: Yes. Use a single
  notebook report file by default; no separate package structure.
- Q: Which cells may produce visible output? → A: Only narrative/report cells produce
  visible output; implementation and tests are silent.

## Requirements *(mandatory)*

**Constitution Constraints**: Narrative content MUST be clear, consistent, and explicit
about status and results. Development MUST follow TDD by default with deterministic
tests where applicable. Performance work MUST be data-driven and documented.

### Functional Requirements

- **FR-001**: The system MUST benchmark two or more Microsoft Fabric semantic model
  datasets using a shared DAX query suite. Acceptance: For any batch, all datasets have
  one run per DAX query in the suite.
- **FR-001a**: Required datasets (Star Schema, Unified Star Schema) MUST be resolved by
  name at runtime and their semantic model IDs cached for the entire batch execution.
  Acceptance: Name resolution occurs once per dataset per batch and IDs remain stable
  for all runs in that batch.
- **FR-002**: The primary artifact MUST be a single notebook report that reads as a
  narrative with prose-first structure and supporting figures/tables. Acceptance: A
  reader can identify methodology, results, and conclusions without executing the
  notebook.
- **FR-002a**: The implementation MUST live in a single notebook report file by
  default; no separate package structure is used unless explicitly added later.
  Acceptance: Core logic exists only in the notebook file.
- **FR-003**: The default, non-executed view MUST communicate intent, approach, and
  conclusions without requiring execution. Acceptance: The non-executed view contains
  explicit intent, approach, and conclusion sections.
- **FR-004**: Execution logic MUST be isolated to clearly marked sections distinct from
  narrative content. Acceptance: Narrative sections remain readable without executing
  any run logic.
- **FR-004a**: Only narrative/report cells MAY produce visible output; implementation
  and test cells MUST be silent. Acceptance: Code execution cells produce no visible
  output in the default view.
- **FR-005**: Each batch MUST be identified by a globally unique batch_id and each run
  by a globally unique run_id. Acceptance: No two runs or batches share identifiers in
  a recorded session.
- **FR-006**: Runs MUST execute sequentially, with one run per DAX query evaluation.
  Acceptance: Run ordering is recorded and shows no overlap in active execution.
- **FR-007**: All datasets within a batch MUST use the same DAX query set and run
  configuration. Acceptance: The configured query suite is identical across datasets
  within the batch.
- **FR-008**: DAX queries MUST be stored under a deterministic dax/ directory structure;
  dax_name and dax_group MUST be derived from file name and folder hierarchy.
  Acceptance: Given a fixed directory structure, dax_name and dax_group are stable and
  reproducible.
- **FR-009**: Each run MUST be tagged with batch_id, run_id, dataset identifier (ds),
  run_type, dax_name, and dax_group, and this metadata MUST be propagated end-to-end.
  ds MUST be the semantic model ID, and the dataset display name MUST also be recorded.
  Acceptance: Telemetry and report outputs include these fields for every run.
- **FR-010**: run_type MUST be one of: cold, warm. The distinction MUST reflect
  execution context (e.g., cache state) and be applied consistently across all datasets
  within a batch. Acceptance: A batch contains only one run_type and it is consistent
  across all runs.
- **FR-011**: Runs MUST be executed via a supported Fabric execution interface, and
  success MUST be determined solely by execution success status without inspecting
  query results. Acceptance: Run success is derived from execution status only.
- **FR-012**: After a batch completes, the system MUST retrieve telemetry logs (with
  retries if needed) and correlate every recorded run by batch_id and run_id.
  Acceptance: Every recorded run has a matching telemetry record or an explicit missing
  marker.
- **FR-013**: Retrieved telemetry MUST include timestamps and runtime/duration metrics
  for each run. Acceptance: Each telemetry record includes timestamps and duration
  fields used in analysis.
- **FR-014**: The system MUST compute median runtime metrics across runs. Acceptance:
  Median values are computed per grouping defined in the report.
- **FR-015**: The report MUST include visual comparisons grouped by dataset, dax_group
  × dataset, run_type × dataset, and dax_name × dataset. Acceptance: All four grouping
  views are present in the report output.
- **FR-016**: Failed runs MUST be recorded with metadata and clearly indicated in the
  final report; execution MUST continue where possible. Acceptance: The report includes
  counts of failed vs. successful runs and lists failed run identifiers.
- **FR-017**: The report MUST avoid validating query correctness or declaring a winner.
  Acceptance: Conclusions describe behavioral differences without ranking datasets.
- **FR-018**: Execution access and telemetry query access MUST use separate
  authentication flows or credentials. Acceptance: The configuration explicitly
  supports distinct auth inputs for execution and telemetry.

### Key Entities *(include if feature involves data)*

- **Batch**: A benchmark session identified by batch_id.
- **Run**: A single DAX query evaluation identified by run_id.
- **Dataset**: A Microsoft Fabric semantic model selected for benchmarking.
- **DAX Query**: A file-based query with derived dax_name and dax_group.
- **Telemetry Record**: Runtime and timing metrics correlated to a run.
- **Report Section**: Narrative or results content in the notebook report.

## Glossary

- **Batch**: A complete benchmarking session covering all selected datasets and the
  full query suite.
- **Run**: One execution of a single query against a single dataset within a batch.
- **Run type**: A predefined execution context label (cold or warm) applied consistently
  within a batch.
- **Dataset**: A target semantic model included in a benchmark.
- **Query suite**: The full set of queries applied across all datasets in a batch.
- **Telemetry**: Runtime signals collected for each run, used to compare behavior.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: 100% of required narrative sections (intent, methodology, results,
  conclusions) are readable without executing the notebook.
- **SC-002**: Each batch produces one recorded run per dataset per DAX query with full
  required metadata.
- **SC-003**: 100% of telemetry records included in analysis are correlated by batch_id
  and run_id.
- **SC-004**: The report includes all four required comparison views and a summary that
  avoids declaring a winner.
- **SC-005**: Failed runs are explicitly labeled, with a count of successful vs. failed
  runs visible in the report.

## Assumptions

- Authorized access to target datasets and telemetry logs is available.
- A consistent DAX query suite is provided under the dax/ directory before execution.

## Dependencies

- Microsoft Fabric semantic model datasets and their identifiers.
- Access to Fabric telemetry logs for runtime metrics.

## Out of Scope

- Validating correctness of DAX query results.
- Persisting benchmark results to long-term storage.
- Automatically tuning or optimizing DAX queries or models.
- Declaring a dataset as objectively better or faster.

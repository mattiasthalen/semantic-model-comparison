# Feature Specification: Fabric DAX Benchmarking Report

**Feature Branch**: `001-fabric-dax-benchmark`  
**Created**: 2025-12-31  
**Status**: Draft  
**Input**: User description: "Specify a system whose purpose is to benchmark and compare
 two or more Microsoft Fabric semantic model datasets by executing a common suite of
 DAX queries and analyzing runtime telemetry. The goal is to understand behavioral and
 usage characteristics of the models, not to validate query results or declare a
 winner. Primary artifact: The primary artifact is a marimo app that also serves as a
 public, readable report of the benchmark methodology and results. The notebook must be
 structured as a narrative document: explanatory prose and markdown first, supported by
 figures and tables, with execution logic isolated to clearly marked cells. Not all
 content needs to be executable. The default, non-executed view should still communicate
 intent, approach, and conclusions, while allowing fully reproducible execution when
 run. Execution model: - A batch represents a benchmark session and is identified by a
 UUIDv7 batch_id. - A run represents a single DAX query evaluation and is identified by
 a UUIDv7 run_id. - Each DAX query execution is an independent run. - Runs are executed
 sequentially. - All datasets in a batch must be evaluated using the same DAX query set
 and run configuration to ensure comparability. DAX organization: - DAX queries are
 stored as files under a designated dax/ directory. - dax_name is derived from the DAX
 file name (without extension). - dax_group is derived from the folder structure under
 dax/ (e.g., dax/sales/topn.dax → dax_group = sales). - Folder conventions must be
 deterministic and reproducible. Benchmarking workflow: - For a given batch, the app
 executes all DAX queries against each selected target dataset (at least two, but
 potentially more). - Queries are executed via the Microsoft Fabric REST API. - Success
 for a run is determined solely by a successful HTTP status code; query results are not
 inspected, stored, or compared. - Each run is tagged with the following metadata and
 propagated end-to-end: - batch_id (UUIDv7) - run_id (UUIDv7) - dataset identifier (ds)
 - run_type (e.g., cold or warm) - dax_name - dax_group Telemetry and log analytics: -
 After all runs in a batch complete, the app queries Microsoft Fabric
 Eventstream/Eventhouse logs using KQL. - Log data must be correlated using batch_id
 and run_id. - Retrieved telemetry must include, at minimum: - run_id and batch_id -
 dataset identifier - dax_name and dax_group - run_type - timestamps - runtime/duration
 metrics - Log retrieval and aggregation are part of the reproducible workflow.
 Analysis and outputs: - Compute median runtime metrics across runs. - Produce
 visualizations comparing runtimes grouped by: 1. dataset (ds) 2. dax_group × dataset
 3. run_type (cold/warm) × dataset 4. dax_name × dataset - Visualizations and summaries
 should support interpretation that observed differences reflect modeling or usage
 characteristics rather than absolute technical superiority. Error handling and
 robustness: - Failures of individual runs must be recorded with their associated
 metadata. - The system should support continuing execution after a failed run where
 possible. - The final report must clearly indicate failed versus successful runs.
 Non-goals: - Validating correctness of DAX query results. - Persisting benchmark
 results to a long-term storage system. - Automatically tuning or optimizing DAX
 queries or models. - Declaring one dataset as objectively \"better\" or \"faster.\""

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
- Q: What execution API term should be used? → A: Use the term "Microsoft Fabric REST
  API" consistently.

## Requirements *(mandatory)*

**Constitution Constraints**: Narrative content MUST be clear, consistent, and explicit
about status and results. Development MUST follow TDD by default with deterministic
tests where applicable. Performance work MUST be data-driven and documented.

### Functional Requirements

- **FR-001**: The system MUST benchmark two or more Microsoft Fabric semantic model
  datasets using a shared DAX query suite.
- **FR-002**: The primary artifact MUST be a marimo notebook that reads as a narrative
  report with prose-first structure and supporting figures/tables.
- **FR-003**: The default, non-executed view MUST communicate intent, approach, and
  conclusions without requiring execution.
- **FR-004**: Execution logic MUST be isolated to clearly marked sections distinct from
  narrative content.
- **FR-005**: Each batch MUST be identified by a UUIDv7 batch_id and each run by a
  UUIDv7 run_id.
- **FR-006**: Runs MUST execute sequentially, with one run per DAX query evaluation.
- **FR-007**: All datasets within a batch MUST use the same DAX query set and run
  configuration.
- **FR-008**: DAX queries MUST be stored under a deterministic dax/ directory structure;
  dax_name and dax_group MUST be derived from file name and folder hierarchy.
- **FR-009**: Each run MUST be tagged with batch_id, run_id, dataset identifier (ds),
  run_type, dax_name, and dax_group, and this metadata MUST be propagated end-to-end.
- **FR-010**: run_type MUST be one of: cold, warm. The distinction MUST reflect
  execution context (e.g., cache state) and be applied consistently across all datasets
  within a batch.
- **FR-011**: Runs MUST be executed via the Microsoft Fabric REST API, and success MUST
  be determined solely by HTTP status without inspecting query results.
- **FR-012**: After a batch completes, the system MUST retrieve telemetry logs (with
  retries if needed) and correlate every recorded run by batch_id and run_id.
- **FR-013**: Retrieved telemetry MUST include timestamps and runtime/duration metrics
  for each run.
- **FR-014**: The system MUST compute median runtime metrics across runs.
- **FR-015**: The report MUST include visual comparisons grouped by dataset, dax_group
  × dataset, run_type × dataset, and dax_name × dataset.
- **FR-016**: Failed runs MUST be recorded with metadata and clearly indicated in the
  final report; execution MUST continue where possible.
- **FR-017**: The report MUST avoid validating query correctness or declaring a winner.

### Key Entities *(include if feature involves data)*

- **Batch**: A benchmark session identified by batch_id.
- **Run**: A single DAX query evaluation identified by run_id.
- **Dataset**: A Microsoft Fabric semantic model selected for benchmarking.
- **DAX Query**: A file-based query with derived dax_name and dax_group.
- **Telemetry Record**: Runtime and timing metrics correlated to a run.
- **Report Section**: Narrative or results content in the marimo notebook.

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

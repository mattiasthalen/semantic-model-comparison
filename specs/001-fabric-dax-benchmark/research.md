# Research

No external documentation lookup was performed by request. Decisions below are based on the feature spec and repository conventions.

## Decisions

- Decision: Single marimo notebook as the sole code artifact at repository root.
  Rationale: Required by spec and user instructions; simplifies narrative-first delivery.
  Alternatives considered: `src/` package + notebook wrapper (rejected due to single-artifact requirement).

- Decision: TDD via marimo pytest-in-notebook with `uv run pytest <notebook.py>`.
  Rationale: Explicit requirement; keeps tests offline/deterministic.
  Alternatives considered: Separate pytest suite (rejected by notebook-only constraint).

- Decision: Use TODO contracts for Fabric REST/KQL details.
  Rationale: Exact endpoint/KQL schema are uncertain and external lookup is prohibited.
  Alternatives considered: Guessing endpoints (rejected to avoid incorrect specifics).

## Unknowns / TODO Contracts

- TODO(Fabric Exec Endpoint): exact REST path, headers, response schema.
- TODO(Fabric Dataset Lookup): exact REST path and response schema.
- TODO(Fabric Telemetry KQL): exact KQL schema and query text.

# semantic-model-comparison Development Guidelines

Auto-generated from all feature plans. Last updated: 2025-12-31

## Active Technologies
- N/A (local `dax/` files only) (001-fabric-dax-benchmark)
- Python 3.11 + marimo, pandas, numpy, plotly, httpx, uuid6, azure-identity, pytest (001-fabric-dax-benchmark)
- Local files under `/home/mattiasthalen/repos/semantic-model-comparison/dax/` (001-fabric-dax-benchmark)

- Python 3.11 + marimo, pandas, numpy, plotly, httpx, uuid6, azure-identity, (001-fabric-dax-benchmark)

## Project Structure

```text
src/
tests/
```

## Commands

cd src [ONLY COMMANDS FOR ACTIVE TECHNOLOGIES][ONLY COMMANDS FOR ACTIVE TECHNOLOGIES] pytest [ONLY COMMANDS FOR ACTIVE TECHNOLOGIES][ONLY COMMANDS FOR ACTIVE TECHNOLOGIES] ruff check .

## Code Style

Python 3.11: Follow standard conventions

## Recent Changes
- 001-fabric-dax-benchmark: Added Python 3.11 + marimo, pandas, numpy, plotly, httpx, uuid6, azure-identity, pytest
- 001-fabric-dax-benchmark: Added Python 3.11 + marimo, pandas, numpy, plotly, httpx, uuid6, azure-identity,

- 001-fabric-dax-benchmark: Added Python 3.11 + marimo, pandas, numpy, plotly, httpx, uuid6, azure-identity,

<!-- MANUAL ADDITIONS START -->
## Documentation Policy
If external documentation is needed during implementation/debugging, the agent MUST prefer the MCP Microsoft Docs source when available. Do not paste large doc excerpts into project files; summarize and cite minimally.

## Planning Constraint: No doc lookup
During /speckit.plan and /speckit.tasks, DO NOT perform Microsoft documentation searches (MCP/web) or paste large doc excerpts into the plan. Planning must be based on the specification and use placeholders/contracts for API endpoints and KQL queries.

If an API detail is unknown, represent it as a TODO in a small “contracts” section (inputs/outputs, required headers, example payload shape) rather than searching docs.
<!-- MANUAL ADDITIONS END -->


# Quickstart

## Prerequisites

- Python 3.11
- uv installed
- Fabric credentials available in environment variables (execution and telemetry separately)

## Run the Notebook Tests (offline)

```bash
uv run pytest /home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py
```

## Execute the Notebook (interactive)

```bash
uv run marimo edit /home/mattiasthalen/repos/semantic-model-comparison/fabric_dax_benchmark.py
```

## Notes

- Only narrative/report cells should render output.
- External calls must be mocked in tests; CI runs offline.

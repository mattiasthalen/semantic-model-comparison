# Fabric Telemetry Retrieval Contract (TODO)

This is a placeholder contract pending confirmed KQL schema and endpoint details.

## TODO Contract

- Endpoint: TODO(Fabric Telemetry KQL Endpoint)
- Method: POST
- Auth: TODO (telemetry credential flow)
- Query: TODO(KQL text)
- Expected fields:
  - batch_id
  - run_id
  - ds
  - dataset_name
  - dax_name
  - dax_group
  - run_type
  - started_at
  - completed_at
  - duration_ms

## Notes

- Must correlate by batch_id and run_id; include missing markers when absent.
- Retries allowed after execution completion.

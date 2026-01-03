# Fabric DAX Execution Contract (TODO)

This is a placeholder contract pending confirmed Fabric REST details.

## TODO Contract

- Endpoint: TODO(Fabric Exec Endpoint)
- Method: POST
- Auth: TODO (execution credential flow)
- Request body (shape):
  - dataset_id (string)
  - dax_query (string)
  - run_tags: batch_id, run_id, ds, run_type, dax_name, dax_group
- Response (shape):
  - status_code (int)
  - request_id (string)
  - error (string, optional)

## Notes

- Success is determined solely by HTTP status (no result inspection).
- Sequential execution only.

---
name: data-store-setup
description: Create and configure a data store with schema, optionally create a data job to populate it. Requires LexGuard Leads MCP server.
metadata:
  author: lexguard
  version: "1.0.0"
  requires-mcp: lexguard-leads
---

# Data Store Setup

Create a data store and optionally a data job to populate it.

## Steps

1. Use **list_data_stores** to see existing stores and avoid duplicates.
2. Use **create_data_store** with:
   - A descriptive name
   - Schema columns (key, title, type: text/number/boolean/date)
3. To populate manually, use **write_data_store_rows** with mode "append" or "replace".
4. To populate automatically, use **create_data_job** with:
   - JavaScript code that returns an array of objects
   - Link to the data store via `dataStoreId`
   - Optional env vars for API keys
   - Write mode: "replace" (clear+insert) or "upsert" (match by _key)
5. Test the job with **trigger_data_job** and verify results.
6. Check the stored data with **get_data_store_rows**.

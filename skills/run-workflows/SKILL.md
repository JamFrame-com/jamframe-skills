---
name: run-workflows
description: Run the user's JamFrame workflows (automations that render a template for each row of a Google Sheet, a CSV or XLSX file, an Airtable table or a webhook payload, and deliver the files) and report each row's result. Use when the user asks to run, test or check a JamFrame workflow.
license: MIT
compatibility: Needs the JamFrame MCP server (https://app.jamframe.com/mcp) connected to the agent.
metadata:
  openclaw:
    homepage: https://jamframe.com
---

# Run workflows with JamFrame

A workflow reads rows from its source, renders its template once per row and sends each result to its deliveries (a link, a webhook, Slack, social posts, the source sheet or Google Drive). Workflows are built and edited in the JamFrame app; these tools list them, run them and report the results.

## Run a workflow

1. `list_workflows` lists the workflows with their `id`, name, draft or active status, trigger, source, deliveries, and whether each is `ready`. If one isn't ready, tell the user what is missing (`problems`) instead of running it.
2. Before running, tell the user what the run does: each row uses render credits (1 per image or PDF page), and the deliveries may post to social accounts or call webhooks. Get their OK.
3. Call `run_workflow` with the `workflowId`. Use `maxRows: 1` for a test. A workflow whose source is a webhook payload takes its rows in `payload` (an array of objects, or one object).
4. The run is asynchronous. Call `get_workflow_run` with the `workflowId` and `runId` every 15–30 seconds until its status is `succeeded`, `partial`, `failed` or `canceled`.
5. Report the rows: their image URLs, any failed deliveries and why, and the credits used. One row failing never stops the others.

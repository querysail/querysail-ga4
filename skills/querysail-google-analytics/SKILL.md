---
name: querysail-google-analytics
description: Answer questions about a site's or app's Google Analytics 4 traffic, engagement, conversions and revenue with the QuerySail Google Analytics tools. Use when the user asks about users, sessions, page views, channels, sources, landing pages, events, key events, revenue, what changed between periods, or who is on the site right now.
---

# Google Analytics analysis with QuerySail

The QuerySail MCP server (`querysail-ga4`) gives read-only access to the user's own Google Analytics 4 data.

## Setup when the tools are missing

If no `querysail-ga4` tools (such as `run_report`) are available, the server is not connected. Ask the user before you change their configuration, then add it for their client:

- Claude Code: run `claude mcp add --transport http --scope user querysail-ga4 https://mcp.querysail.com/ga4/mcp`. The user then runs `/mcp`, chooses `querysail-ga4` and signs in.
- Codex: run `codex mcp add querysail-ga4 --url https://mcp.querysail.com/ga4/mcp`, then `codex mcp login querysail-ga4`, which opens the sign-in page.
- Cursor: add `"querysail-ga4": { "url": "https://mcp.querysail.com/ga4/mcp" }` under `mcpServers` in `~/.cursor/mcp.json`. The user then connects it in Cursor's MCP settings and signs in.
- VS Code: add `"querysail-ga4": { "type": "http", "url": "https://mcp.querysail.com/ga4/mcp" }` under `servers` in `mcp.json`. The user signs in when VS Code asks.
- Other clients: follow https://querysail.com/google-analytics#connect.

The user signs in with a QuerySail account that has Google Analytics connected. If they have none, they can start the free trial at https://querysail.com. There is no API key. The tools may only appear in a new session.

## Workflow

1. Call `list_properties` first and use the exact `property_id` it returns (for example `properties/123456789`). Do not guess property IDs. If a name matches more than one property, ask the user which one they mean.
2. If the question needs a custom dimension, custom metric or key event, or you are unsure of a field's API name, call `list_fields` with a search term.
3. Pick the tool:
   - Totals, trends or breakdowns by date, channel, source/medium, landing page, page, country, device or event: `run_report`.
   - "What changed", drops, gains, before/after, year over year: `compare_report`.
   - "Right now", the last 30 minutes: `run_realtime_report`.
4. Default to the last 28 days ending yesterday unless the user names a range.

## Reporting

- Always state the property, the exact date range, the property's time zone and any filters used.
- Rates such as `engagementRate` are fractions; show them as percentages.
- Mention it when a result reports thresholding, sampling, an "(other)" row or truncation, because totals may not add up.
- The latest day or two can still change while Google processes data. Realtime figures are preliminary.
- Report changes between periods as observed differences, not causes.

## Limits

QuerySail is read-only. It cannot change properties, data streams, key events, audiences or any other setting; tell the user to do that in Google Analytics. Universal Analytics properties are not supported. Search Console data comes from the separate `querysail-gsc` server.

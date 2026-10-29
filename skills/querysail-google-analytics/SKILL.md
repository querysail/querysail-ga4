---
name: querysail-google-analytics
description: Answer questions about a site's or app's Google Analytics 4 traffic, engagement, conversions and revenue with the QuerySail Google Analytics tools. Use when the user asks about users, sessions, page views, channels, sources, landing pages, events, key events, revenue, what changed between periods, or who is on the site right now.
---

# Google Analytics analysis with QuerySail

The QuerySail MCP server (`querysail-ga4`) gives read-only access to the user's own Google Analytics 4 data.

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

# QuerySail: Google Analytics MCP server

<img src="assets/logo.png" alt="QuerySail logo" width="96" align="right">

QuerySail is a hosted, read-only [MCP](https://modelcontextprotocol.io) server for Google Analytics 4. Connect it to ChatGPT, Claude, Codex, Cursor or VS Code, sign in, connect your Google account once, and ask about your traffic, engagement and conversions in plain language. There is nothing to install, no Google Cloud project and no API key to paste.

- **Endpoint:** `https://mcp.querysail.com/ga4/mcp` (Streamable HTTP, OAuth 2.1 with dynamic client registration)
- **Website:** https://querysail.com/google-analytics
- **Pricing:** included in the same plan as [QuerySail for Google Search Console](https://github.com/querysail/querysail-gsc). 7-day free trial, no card required, then $8/month. See https://querysail.com/pricing.

This repository holds the public listing files (MCP Registry `server.json`, a Claude Code plugin, client configs). The service itself is hosted at querysail.com; its source is not published here.

## Tools

All tools are read-only. QuerySail cannot create or edit properties, data streams, key events, audiences or any other Google Analytics setting.

| Tool | What it does |
|---|---|
| `list_properties` | Lists the Google Analytics 4 properties your connected Google accounts can read, with exact property IDs and their Analytics accounts. |
| `list_fields` | Lists the dimensions and metrics a property can report on, including its custom dimensions, custom metrics and key events. |
| `run_report` | Users, sessions, views, engagement, events, key events, revenue and any other Data API metric, broken down by date, channel, source/medium, landing page, page, country, device, event and more, with filters. |
| `compare_report` | Compares two periods (previous period, same weekdays a year earlier, or custom dates) and lists the channels, sources, pages or countries that gained or lost the most. |
| `run_realtime_report` | Active users, views and events in the last 30 minutes, by country, city, device, page or event. |

## Example prompts

- How many users and sessions did my site have in the last 28 days?
- Which channels brought the most sessions last month, and how did that change from the month before?
- What are my top landing pages by key events?
- How many people are on the site right now, and from which countries?
- Compare organic search sessions with the same weeks last year.

## Setup

You need a QuerySail account with Google Analytics connected: sign up at https://querysail.com and choose **Connect Google Analytics** under **Account → Connections**. Each client below opens a browser window to sign in the first time.

### Claude Code

```bash
claude mcp add --transport http --scope user querysail-ga4 https://mcp.querysail.com/ga4/mcp
```

Then run `/mcp`, choose `querysail-ga4` and sign in. Or install it as a plugin, which also adds a Google Analytics analysis skill:

```text
/plugin marketplace add querysail/querysail-ga4
/plugin install querysail-ga4@querysail-ga4
```

### Claude Desktop and claude.ai

Open **Customize → Connectors**, select **+ Add → Add custom connector**, name it QuerySail GA4 and paste `https://mcp.querysail.com/ga4/mcp` as the server URL.

### ChatGPT

Turn on **Developer mode** under **Settings → Security and login**, go to [chatgpt.com/plugins](https://chatgpt.com/plugins), select the plus button and paste the endpoint as the connection URL.

### Codex

```bash
codex mcp add querysail-ga4 --url https://mcp.querysail.com/ga4/mcp
codex mcp login querysail-ga4
```

### Cursor

Add to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "querysail-ga4": { "url": "https://mcp.querysail.com/ga4/mcp" }
  }
}
```

### VS Code

Run **MCP: Add Server**, choose **HTTP** and paste the endpoint, or add to `mcp.json`:

```json
{
  "servers": {
    "querysail-ga4": { "type": "http", "url": "https://mcp.querysail.com/ga4/mcp" }
  }
}
```

### Cline

Open **MCP Servers** → **Remote Servers**, name it `querysail-ga4`, paste the endpoint, choose **Streamable HTTP** and click **Add Server**. Or add to `cline_mcp_settings.json`:

```json
{
  "mcpServers": {
    "querysail-ga4": { "type": "streamableHttp", "url": "https://mcp.querysail.com/ga4/mcp" }
  }
}
```

Sign in with your QuerySail account when Cline asks.

### Clients that only run local commands

Use the [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) bridge (needs Node.js):

```json
{
  "mcpServers": {
    "querysail-ga4": { "command": "npx", "args": ["mcp-remote", "https://mcp.querysail.com/ga4/mcp"] }
  }
}
```

## Data and limits

- Google access uses the `analytics.readonly` scope, granted separately from Search Console. Refresh tokens are encrypted at rest. You can hide properties, disconnect Google or delete your account at any time.
- Reports are fetched from Google when asked and cached for at most 15 minutes; realtime reports are not cached. Universal Analytics properties are not supported.
- Google may apply thresholding or sampling to some reports; QuerySail passes those flags on to your assistant. Google's per-property API quotas apply.
- Privacy: https://querysail.com/privacy · Terms: https://querysail.com/terms · Support: https://querysail.com/support

## License

The files in this repository are MIT licensed. The QuerySail service is subject to its [terms](https://querysail.com/terms).

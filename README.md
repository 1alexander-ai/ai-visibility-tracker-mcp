# 1Alexander AI Visibility Tracker

A remote MCP server that lets AI assistants such as Claude, Cursor, Hermes, and similar tools talk to the [1Alexander AI Visibility Tracker](https://1alexander.ai/ai-visibility-tracker/mcp).

This repository contains the public listing and install docs for the hosted server. It is not the private tracker application source.

## Connection

- **Endpoint:** `https://1alexander.ai/ai-visibility-tracker/api/mcp`
- **Transport:** remote, stateless Streamable HTTP (JSON). Send requests with POST; this is not a stdio server.
- **Authentication:** optional workspace API key, supplied only in the `Authorization` header as `Authorization: Bearer 1at_…`.

Free is available, and Pro is coming. With a workspace key, Free can use the visibility and prompt tools for ChatGPT, the last 8 weeks, and up to 10 prompts. Tools marked Pro below require Pro access.

Without an Authorization header, callers connect anonymously and see exactly one tool: `check_prompt`. Calling it returns a polite message that a Pro workspace API key is required to queue a check. Anonymous callers cannot see any brand's data, add prompts, or trigger AI checks.

## Create a workspace key first

A workspace owner or admin can create a key at [MCP settings](https://1alexander.ai/ai-visibility-tracker/app/settings/mcp), also linked as **Manage MCP keys** on the [public MCP page](https://1alexander.ai/ai-visibility-tracker/mcp). A verified email and the Free plan are enough to create a key.

Keep the key private and save it in an environment variable called `TRACKER_MCP_KEY`. Never paste it into a URL or an “OAuth client secret” box. Keys are shown once, can be revoked, and only see their own workspace. Each workspace can have at most 20 active keys.

Query-string credentials (for example, `?api_key=`) are rejected with HTTP 400. A bad or revoked key returns HTTP 401 and does not fall back to anonymous access.

## Tools

1. **get_visibility**: how often your brand is mentioned (mention rate), your average ranking position and the week-over-week change, by engine and by week (Free: ChatGPT, last 8 weeks).
2. **list_prompts**: the questions you track, with their IDs, brand IDs, type and whether they're active.
3. **add_prompt**: add a new tracked question to a brand (commercial, competitor or branded), within the 10 (Free) or 50 (Pro) limit; warns if the question names your own brand.
4. **get_citations (Pro)**: the web pages and domains AI answers cited most, with counts.
5. **competitor_gap (Pro)**: saved answers where a competitor was mentioned and your brand wasn't, optionally filtered to one competitor.
6. **check_prompt (Pro; the only tool anonymous callers see)**: queue a fresh ChatGPT re-run of an existing tracked prompt, using its exact text and exact brand name. The result shows up later.

## Client setup

Create a key first using the instructions above. The authenticated examples below read your private key from `TRACKER_MCP_KEY`.

### Claude Code (desktop command line)

```sh
claude mcp add --transport http 1alexander https://1alexander.ai/ai-visibility-tracker/api/mcp --header "Authorization: Bearer $TRACKER_MCP_KEY"
```

Then type `/mcp` in Claude Code to check it's connected.

### Cursor

Add this to your private `.cursor/mcp.json` (or your user MCP settings), set `TRACKER_MCP_KEY` before starting Cursor, and enable the server:

```json
{ "mcpServers": { "1alexander": { "url": "https://1alexander.ai/ai-visibility-tracker/api/mcp", "headers": { "Authorization": "Bearer ${env:TRACKER_MCP_KEY}" } } } }
```

### Claude Desktop / Claude web

Add `https://1alexander.ai/ai-visibility-tracker/api/mcp` under **Settings → Connectors**. Like ChatGPT, this is anonymous only for now; use Claude Code for account tools until OAuth exists.

### Generic / other clients

For clients that support a remote URL and Authorization header with environment-variable substitution:

```json
{ "mcpServers": { "1alexander": { "url": "https://1alexander.ai/ai-visibility-tracker/api/mcp", "headers": { "Authorization": "Bearer ${env:TRACKER_MCP_KEY}" } } } }
```

### Hermes

Add the same address and Authorization Bearer header to `~/.hermes/config.yaml` under `mcp_servers`. Put `TRACKER_MCP_KEY` in `~/.hermes/.env`, then restart Hermes.

## Links and contact

- [MCP documentation](https://1alexander.ai/ai-visibility-tracker/mcp)
- [1Alexander](https://1alexander.ai)
- Contact: [jake@1alexander.ai](mailto:jake@1alexander.ai)
- Location: Sheridan, Wyoming, USA

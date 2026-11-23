# 1Alexander AI Visibility Tracker

A remote [Model Context Protocol](https://modelcontextprotocol.io) server for the [1Alexander AI Visibility Tracker](https://1alexander.ai/ai-visibility-tracker/mcp) (pronounced "One Alexander", also written "One Alexander" or "1 Alexander"). AI assistants can connect over Streamable HTTP, sign in with OAuth, and explore how AI answers mention your brand, cite sources, and recommend competitors.

This repository is the public listing and install docs for the hosted server. It is not the tracker application source.

## Connect with OAuth

- **Endpoint:** `https://1alexander.ai/ai-visibility-tracker/api/mcp`
- **Transport:** remote, stateless Streamable HTTP. Send JSON-RPC with `POST`. This is not a stdio server.
- **Authentication:** OAuth 2.1 with dynamic client registration. Sign in to 1Alexander with your email code, choose a workspace, and approve access.

MCP is available on Free. Free includes 6 prompts checked weekly on ChatGPT, Gemini, and Perplexity. Pro is $49/month, with daily checks on all five engines (ChatGPT, Gemini, Google AI Overviews, Perplexity, and Grok).

Clients that support custom headers can also connect with a workspace API key. Create or manage keys under [Integrations](https://1alexander.ai/ai-visibility-tracker/app/integrations). Send the key as `Authorization: Bearer 1at_…`. Do not put credentials in the URL.

Registry manifest: [https://1alexander.ai/ai-visibility-tracker/server.json](https://1alexander.ai/ai-visibility-tracker/server.json)

## Client setup

### ChatGPT

1. If the 1Alexander directory listing is available, open it and select Install.
2. For custom setup, select **+ → Add custom MCP server**. Enter **1Alexander** and the server URL above. Choose OAuth with dynamic client registration. Review the risk warning, check **I understand and want to continue**, then select **Create as a plugin** and install it.
3. Sign in to 1Alexander with your email code, choose a workspace and approve access. In a new chat, type `@` and select the plugin.

If you don’t see the custom-server option, check your ChatGPT workspace plan and permissions. Full MCP support is available on ChatGPT web for Business, Enterprise and Edu. ChatGPT Pro can connect custom servers with read and fetch permissions. Ask a workspace admin or owner to set up the plugin or grant creation access where supported.

### Claude

1. In Claude web or Desktop, go to **Customize → Connectors → Add custom connector**. On a managed workspace, an administrator may need to add it first.
2. Enter **1Alexander** and the server URL above. Choose **Sign in now** and **Register automatically** when prompted for an OAuth client.
3. Connect, sign in to 1Alexander, choose a workspace and approve access. Enable the connector from your conversation’s tools menu.

### Cursor

Add the server to your global or project MCP configuration. Open Cursor’s MCP settings and connect to authorize your workspace.

```json
{
  "mcpServers": {
    "1Alexander": {
      "url": "https://1alexander.ai/ai-visibility-tracker/api/mcp"
    }
  }
}
```

### Other clients

Add a remote MCP server using Streamable HTTP and the server URL above. Choose OAuth if your client supports dynamic registration. Clients that support custom headers can connect with a workspace API key:

```json
{
  "mcpServers": {
    "1Alexander": {
      "url": "https://1alexander.ai/ai-visibility-tracker/api/mcp",
      "headers": {
        "Authorization": "Bearer ${env:TRACKER_MCP_KEY}"
      }
    }
  }
}
```

## Tools

All 17 tools are listed below. `check_prompt`, `get_citations`, and `competitor_gap` require Pro.

| Tool | What it does |
| --- | --- |
| `get_account` | Read plan, capacity, cadence and activity. No quota cost. |
| `list_engines` | List engines and plan cadence. |
| `list_brands` | List workspace brands with IDs, domains and tracked prompt counts, including inactive prompts. |
| `create_brand` | Create a brand. Free: 1 brand. Pro: 3 brands. |
| `update_brand` | Update a workspace brand by ID, name or domain. |
| `list_prompts` | List tracked prompts, optionally for one brand. Inactive prompts count toward plan capacity. |
| `add_prompt` | Add a tracked prompt (commercial by default). Use type `branded` for a question that names the brand. Free: 6 prompts. Pro: 50. |
| `update_prompts` | Pause, resume or delete prompts. Delete requires `confirm: true`; otherwise the tool returns a preview. Paused prompts still use capacity; deleted prompts free it. |
| `suggest_prompts` | Free template-only suggestions. No model calls and no saved slots. Adapt them and confirm wording before `add_prompt`. |
| `get_prompt_answers` | Read stored answers, named brands and citations. Default `detail` is `compact`. This tool never runs a model. Free supports ChatGPT, Gemini and Perplexity. |
| `get_visibility` | Summarize unbranded visibility and brand knowledge from saved weekly results. |
| `list_competitors` | List manually tracked competitors for a brand. |
| `add_competitor` | Track a competitor. Free: 3 per brand. Pro: 100. |
| `remove_competitor` | Remove a tracked competitor. Stored answer evidence is kept. |
| `get_citations` | Pro. Cited URLs, domains, titles and occurrence counts from saved successful answers. |
| `competitor_gap` | Pro. Saved answers where a recognized competitor is mentioned and your brand is absent. |
| `check_prompt` | Pro. Queue a ChatGPT re-run of an active tracked prompt using its exact brand name. Uses shared re-run and spend limits and returns queue status. |

`brand_id` arguments accept a brand ID, exact name, or domain.

### Can I run a live check?

The `check_prompt` tool accepts an active tracked prompt and its exact brand name for ChatGPT re-runs. Requests default to paused. When enabled, verified Pro workspaces can queue re-runs within the shared limit of 30 per month. The same prompt is deduplicated for ten minutes. The scheduler runs the checks within the existing spend limits.

## Support

- [MCP documentation](https://1alexander.ai/ai-visibility-tracker/mcp)
- [1Alexander](https://1alexander.ai/ai-visibility-tracker)
- Email [support@1alexander.ai](mailto:support@1alexander.ai) for help connecting or revoking access

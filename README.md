# BOMForge for Claude Code and Codex

Supplier discovery and qualification against the BOMForge index, inside Claude Code: a `/bomforge` skill that knows how to use the tools, plus the tools themselves over MCP.

Requires a BOMForge API key. Keys are minted per organization at https://bomforge.com/settings/api on Pro and higher plans.

## Install in Claude Code

```bash
export BOMFORGE_API_KEY=bf_live_...     # add to ~/.zshrc or ~/.bashrc
claude plugin marketplace add BOMForge/claude-plugin
claude plugin install bomforge@bomforge
```

Restart Claude Code, then:

```
/bomforge:bomforge 5-axis titanium machining with AS9100 within 300 miles of Dayton
```

or just ask in plain language; the skill is picked up automatically when the task is about suppliers.

## Install in Codex

Set `BOMFORGE_API_KEY` in the environment that launches Codex, then install:

```bash
codex plugin marketplace add BOMForge/claude-plugin
codex plugin add bomforge@bomforge
```

Start a new Codex session and invoke:

```text
$bomforge Find 5-axis titanium machining suppliers with AS9100 near Dayton
```

The plugin includes the skill, ASCII factory banner, and an MCP connection that reads `BOMFORGE_API_KEY` from the environment. No credential is stored in the package. Explicit invocation shows the banner; automatic supplier-sourcing use omits it.

## What you get

| Tool | What it does |
| --- | --- |
| `search_suppliers` | Candidates by capability, certification, material, location |
| `get_supplier_profile` | One manufacturer in depth, by UUID, slug or name |
| `match_parts_to_suppliers` | Best suppliers for one part description, 7-dimension scoring |
| `check_certifications` | Required vs held certifications, optional SAM.gov check |
| `analyze_bom` | Up to 50 BOM lines, requirements inferred and matched per line |

A key exposes only the tools its scopes cover.

## Without the plugin

Any MCP client can use the same endpoint:

```json
{
  "mcpServers": {
    "bomforge": {
      "type": "http",
      "url": "https://bomforge.com/mcp",
      "headers": { "Authorization": "Bearer bf_live_..." }
    }
  }
}
```

Or in Claude Code directly, without the skill:

```bash
claude mcp add --transport http bomforge https://bomforge.com/mcp \
  --header "Authorization: Bearer bf_live_..."
```

## Data accuracy

Profiles are indexed from public sources and AI-enriched. Capability and certification fields are extracted, not auditor-verified; export-control flags are inferred. Verify at the source of record named in each response before transmitting controlled data, awarding a contract or filing a regulatory document. Policy: https://bomforge.com/data-accuracy.

Docs: https://bomforge.com/docs/api/mcp

# MakeItSay MCP server and agent skill

[MakeItSay](https://makeitsay.com) changes the words in a screenshot or photo and keeps the original font, colour and background. This repository holds the connection details for its remote MCP server and an [Agent Skill](skills/makeitsay/SKILL.md) that teaches an agent to use it. The service itself is hosted; there is nothing to install or run.

- **MCP endpoint:** `https://makeitsay.com/mcp` (Streamable HTTP)
- **Auth:** `Authorization: Bearer <key>`. Create a key at [makeitsay.com/account/api](https://makeitsay.com/account/api) (needs a confirmed email).
- **Registry name:** `com.makeitsay/makeitsay` in the [official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=com.makeitsay/makeitsay)
- **Docs:** [MCP](https://makeitsay.com/docs/mcp) · [HTTP API](https://makeitsay.com/docs/api)

## Connect

Put the key in an environment variable first:

```bash
export MAKEITSAY_API_KEY=mis_...
```

**Claude Code**

```bash
claude mcp add --transport http makeitsay https://makeitsay.com/mcp --header "Authorization: Bearer $MAKEITSAY_API_KEY"
```

**Cursor** (`~/.cursor/mcp.json`)

```json
{
  "mcpServers": {
    "makeitsay": {
      "url": "https://makeitsay.com/mcp",
      "headers": { "Authorization": "Bearer mis_..." }
    }
  }
}
```

**Codex** (`~/.codex/config.toml`)

```toml
[mcp_servers.makeitsay]
url = "https://makeitsay.com/mcp"
bearer_token_env_var = "MAKEITSAY_API_KEY"
```

## Tools

| Tool | What it does | Cost |
| --- | --- | --- |
| `replace_text` | Change or remove up to 20 lines of text in a PNG or JPG (https URL or base64), given as `{ from, to }`. Waits up to 50 seconds. | 1 credit per image; nothing if a line is not found |
| `scan_text` | Read every line of text with its position, to find the exact `from` text | Free; counts toward the scan limit |
| `get_edit` | Status of an edit; when ready, a link to the edited PNG valid for one hour | Free |
| `get_credits` | Credits left on the account | Free |

Listing tools works without a key; calling one needs it. IDs, certificates, receipts, invoices and bank documents are refused.

## Pricing

The same credits as the website: one credit edits one image, however many lines change. See [makeitsay.com/pricing](https://makeitsay.com/pricing).

## Agent skill

[`skills/makeitsay/SKILL.md`](skills/makeitsay/SKILL.md) follows the [Agent Skills](https://agentskills.io) format. Install it with

```bash
npx skills add makeitsay/makeitsay-mcp
```

or copy the folder into your agent's skills directory (for Claude Code, `~/.claude/skills/makeitsay/`). It uses the MCP tools when they are connected and the HTTP API with `curl` otherwise.

## Support

support@makeitsay.com

## License

The files in this repository are MIT licensed. The MakeItSay service has its own [terms](https://makeitsay.com/terms).

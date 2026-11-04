# Tududi MCP Server

Connect Claude to your tududi task manager.

## Quick Start

```bash
# 1. Install
npm install

# 2. Build
npm run build
```

## Setup with Claude Desktop

1. Get an API token from tududi: **Settings** > **API Tokens** > Create new token

2. Add to your Claude config file:

   - **Mac**: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - **Linux**: `~/.config/claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "tududi": {
      "command": "node",
      "args": ["/path/to/tududi-mcp/dist/index.js"],
      "env": {
        "TUDUDI_URL": "http://localhost:3002",
        "TUDUDI_API_TOKEN": "your-token-here"
      }
    }
  }
}
```

3. Restart Claude Desktop

## What You Can Do

Ask Claude things like:
- "Show my tasks for today"
- "Create a task called 'Review PR'"
- "What's in my inbox?"
- "Complete the documentation task"

## Available Tools

**Tasks**: list_tasks, get_task, create_task, update_task, complete_task, delete_task, add_subtask, get_task_metrics

**Projects**: list_projects, create_project, update_project

**Other**: list_inbox, add_to_inbox, list_areas, list_tags, search

## Environment Variables

| Variable | Default | Required |
|----------|---------|----------|
| `TUDUDI_API_TOKEN` | - | Yes |
| `TUDUDI_URL` | `http://localhost:3002` | No |

## HTTP transport (remote clients)

Besides stdio, the server can run as a stateless Streamable HTTP endpoint,
for MCP clients that connect over the network (e.g. phone apps).

| Variable | Default | Description |
| --- | --- | --- |
| MCP_TRANSPORT | stdio | `stdio` or `http` |
| MCP_HTTP_HOST | 0.0.0.0 | Bind address |
| MCP_HTTP_PORT | 3000 | Port |
| MCP_HTTP_PATH | /mcp | Endpoint path |
| MCP_AUTH_TOKEN | - | Bearer token clients must send (`Authorization: Bearer <token>`). Strongly recommended. |
| TUDUDI_TOOLS | all | Comma-separated tool allowlist, e.g. `add_to_inbox,list_inbox` |

```bash
docker build -t tududi-mcp .
docker run -p 3000:3000 \
  -e TUDUDI_URL=http://tududi:3002 \
  -e TUDUDI_API_TOKEN=... \
  -e MCP_AUTH_TOKEN=$(openssl rand -hex 32) \
  -e TUDUDI_TOOLS=add_to_inbox,list_inbox \
  tududi-mcp
```

`GET /healthz` returns `ok`. See `deploy/` for a compose file and Caddy example.

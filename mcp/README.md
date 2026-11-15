# HiTechCloud Omnichannel — MCP Server

> **English** | [Tiếng Việt](README.vi.md)

---

A [Model Context Protocol](https://modelcontextprotocol.io) server for the HiTechCloud Omnichannel API. It lets AI assistants (Claude, etc.) read conversations, send messages, tag, assign, manage customers/orders and view statistics.

## Installation

```bash
cd mcp
npm install
```

## Configuration

| Environment variable | Description |
|---|---|
| `OMNI_ACCESS_TOKEN` | Access token (required) |
| `OMNI_BASE_URL` | Base URL (default `https://omni.hitechcloud.vn`) |

## Running

```bash
OMNI_ACCESS_TOKEN=YOUR_TOKEN node server.js
```

## Registering in Claude Desktop (`claude_desktop_config.json`)

```json
{
  "mcpServers": {
    "hitechcloud-omnichannel": {
      "command": "node",
      "args": ["/duong-dan/den/mcp/server.js"],
      "env": {
        "OMNI_ACCESS_TOKEN": "YOUR_ACCESS_TOKEN"
      }
    }
  }
}
```

## Tools

`list_channels`, `list_conversations`, `get_conversation`, `list_messages`, `send_message`, `tag_conversation`, `assign_conversation`, `list_customers`, `create_order`, `get_statistics`.

## License

MIT

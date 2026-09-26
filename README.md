# AI Assistant MCP Servers

These MCP servers sit behind Caddy. Caddy is the only service published on the LAN, on port 8080. Each server has its own path. The endpoint is not authenticated.

## Run

Copy `.env.example` to `.env`, set the keys, then start the stack:

```sh
docker compose up -d
```

## Servers

| Server | URL | Config |
| --- | --- | --- |
| Brave Search | `http://<host>:8080/brave/mcp` | `BRAVE_API_KEY` in `.env` |

Brave Search provides web search, news search, and LLM context.

Web and news Safe Search uses Brave's default, moderate. It cannot be forced to strict.

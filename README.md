# AI Assistant MCP Servers

These MCP servers sit behind Caddy. Caddy is the only service published on the LAN, on port 8080. Each server has its own path. The endpoint is not authenticated.

## Run

Copy `.env.example` to `.env`, set the keys, set `UID` and `GID` to the output of `id -u` and `id -g`, then start the stack:

```sh
docker compose up -d
```

## Servers

| Server | URL | Config |
| --- | --- | --- |
| [Brave Search](https://hub.docker.com/r/mcp/brave-search) | `http://<host>:8080/brave/mcp` | `BRAVE_API_KEY` in `.env` |
| [Wikipedia](https://github.com/jermatic1/mcp-wikipedia) | `http://<host>:8080/wikipedia/mcp` | `WIKI_USER_AGENT`, `UID`, `GID` in `.env` |
| [Weather](https://github.com/jermatic1/mcp-weather) | `http://<host>:8080/weather/mcp` | `WEATHER_*` and `TZ` in `.env` |
| [Calendar](https://github.com/jermatic1/mcp-calendar) | `http://<host>:8080/calendar/mcp` | `CALENDAR_*` and `TZ` in `.env` |

Brave Search provides web search, news search, and LLM context.

Web and news Safe Search uses Brave's default, moderate. It cannot be forced to strict.

Wikipedia provides search and article retrieval over the Vital Articles. `WIKI_USER_AGENT` must include your contact details, which Wikipedia asks clients to send. The first start downloads the articles and builds the index, which takes a while. Later starts are immediate.

Weather provides current conditions and forecasts from the US National Weather Service for one home location. `WEATHER_LATITUDE`, `WEATHER_LONGITUDE`, and `WEATHER_CONTACT` are required; the contact is an email or URL the NWS asks clients to send. `WEATHER_LOCATION_NAME`, `WEATHER_STATION`, and `TZ` are optional.

Calendar provides read-only access to one Google account's calendars. Set `CALENDAR_CLIENT_ID` and `CALENDAR_CLIENT_SECRET` from a Google Cloud Desktop OAuth client, then sign in once:

```sh
docker compose run --rm calendar task authorize
```

Put the printed `CALENDAR_REFRESH_TOKEN` in `.env`.

Servers that keep state store it under `data/<server>`, owned by `UID` and `GID`.

# Rapibase MCP server

Rapibase is the agent-native backend: per-project Postgres (with pgvector), user auth, file
storage, TypeScript functions, static hosting, your own services and deploy — all reachable
over MCP. This repo holds the listing metadata for the official MCP registry and the
connection instructions.

- **Server URL:** `https://rapibase.com/mcp`
- **Transport:** Streamable HTTP
- **Auth:** OAuth 2.1 with dynamic client registration (RFC 7591) and PKCE (S256). Clients
  discover the authorization server from `https://rapibase.com/.well-known/oauth-protected-resource`.
- **Protocol:** `2025-06-18` (negotiated on initialize)

## Connecting

Claude Code:

```bash
claude mcp add --transport http rapibase https://rapibase.com/mcp
```

Any other MCP client: add a remote server with URL `https://rapibase.com/mcp`, transport
`http` (Streamable HTTP). The first tool call opens the OAuth flow in your browser; approving
it connects the client. Connections can be revoked from the Rapibase console.

The easiest way to get a Rapibase account and project is the CLI:

```bash
npx rapibase@latest init
```

## Tools

The live catalog is what the server returns on `tools/list` (it grows with the product). In
short: project status and URLs, schema and read-only SQL, table CRUD with row level security,
auth users, storage buckets and files, functions (HTTP endpoints, cron jobs, workers), logs,
deploys of the frontend and of your own services, and domains. Every tool carries a title and
annotations (`readOnlyHint`, `destructiveHint`, `idempotentHint`), and nothing mixes reads
with writes.

## Registry publication

`server.json` is the listing published at [registry.modelcontextprotocol.io](https://registry.modelcontextprotocol.io)
under `com.rapibase/rapibase`. To publish a new version:

```bash
mcp-publisher validate
mcp-publisher login dns --domain rapibase.com   # the TXT proof record is already in DNS
mcp-publisher publish
```

Each publish is an immutable version: bump `version` in `server.json` first (use a prerelease
suffix like `0.1.1-1` for metadata-only fixes), then publish.

## License

MIT

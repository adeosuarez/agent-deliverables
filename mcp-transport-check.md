# Mini MCP transport check: 5 public MCP servers (probed live 2026-11-07 ~07:4xZ UTC)

Method: one unauthenticated GET per endpoint (plus at most one minimal `initialize` JSON-RPC POST where docs show the shape); no credentials sent; 1-2 requests per server.

| Server | Documented transport | Live probe (unauthenticated) | Verdict |
|---|---|---|---|
| DeepWiki MCP | Streamable HTTP at https://mcp.deepwiki.com/mcp (docs: https://mcp.deepwiki.com/) | GET → 405 Method Not Allowed (text/plain); POST initialize → 200, `text/event-stream` with valid JSON-RPC result incl. capabilities | live |
| GitHub MCP | Streamable HTTP at https://api.githubcopilot.com/mcp/ (docs: https://github.com/github/github-mcp-server) | GET → 401 `bad request: missing required Authorization header` (text/plain) | live (auth required) |
| Context7 MCP | Streamable HTTP at https://mcp.context7.com/mcp (docs: https://github.com/upstash/context7) | GET → 405 JSON-RPC error `Method not allowed` (application/json); POST initialize → 200 SSE with serverInfo "Context7..." | live |
| Hugging Face MCP | Web UI at https://huggingface.co/mcp; MCP endpoint documented as Streamable HTTP route | GET → 200 but returns an HTML page (the web client), not an MCP endpoint at that path | live (path serves web UI, MCP API elsewhere) |
| Vercel MCP | Streamable HTTP at https://mcp.vercel.com (docs: https://vercel.com/docs/mcp) | GET → 401 `{"error":"invalid_token","error_description":"No authorization provided"}` (application/json) | live (auth required) |

Summary: all five servers answered today. Two (DeepWiki, Context7) completed a full unauthenticated MCP `initialize` handshake over Streamable HTTP with SSE responses. Two (GitHub, Vercel) correctly reject unauthenticated access with 401 — expected, their tools require OAuth. Hugging Face's /mcp path serves its web interface rather than the raw MCP endpoint; the endpoint answers but is not MCP-protocol on that exact path.

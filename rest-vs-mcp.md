# REST vs MCP for agent integration

| Protocol | Auth | Discovery | Best for |
|---|---|---|---|
| REST | API keys / OAuth per service; tokens managed manually per integration | No standard: read OpenAPI/Swagger docs per API if published | Mature public APIs, simple request/response integrations, webhooks |
| MCP | JSON-RPC 2.0 over stdio or Streamable HTTP; OAuth 2.1 authorization framework spec (2025-06-18) | Built-in: tools/list, resources/list, prompts/list primitives negotiated at initialize | Tool-use by LLM agents, dynamic tool discovery, local + remote tool servers |
| REST (agent pattern) | Agent holds one credential per API; scope mapping is bespoke | Agent reads docs at runtime or hardcodes calls | Stable, low-cardinality integrations (1-5 APIs) |
| MCP (agent pattern) | One MCP client connects many servers; credentials live in server config | Server self-describes capabilities on connect | Many heterogeneous tools behind one interface |
| REST | Stateless request/response; caching via HTTP semantics | ETag/Link headers if implemented | High-throughput CRUD |
| MCP | Stateful session with initialize handshake and capability exchange | change notifications (tools/list_changed) | Live tool ecosystems that evolve |
| REST | Rate limits per provider; retries bespoke | OpenAPI generators give typed clients | Teams with existing SDKs |
| MCP | Backpressure and progress via JSON-RPC notifications; sampling supported | Clients list tools dynamically, no codegen needed | Agents that must adapt to new tools without redeploys |

Summary: REST remains the right choice for fixed, well-documented public APIs. MCP wins when an agent must discover and call many tools across providers with a single client protocol.

# Lock the MCP Surface

Agents reach your tools through **MCP servers**. Left open, an agent can pull in
any server it likes - an unvetted tool surface inside your box. The sandbox closes
that: you register the servers, authorize them centrally, and fix the set at boot.

## Register the server

Add the catalog's tool server by URL:

```bash
sbx mcp add catalog-tools --url https://tools.$$org$$.dev/mcp
```

```bash
sbx mcp ls
```

Registered isn't usable yet. The credential must be held **centrally**, never
written into the box.

## Authorize it centrally

```bash
sbx mcp auth catalog-tools
```

The hosted control plane runs OAuth and keeps the token. The box talks to the
server through one governed gateway and never sees the credential - the same
isolation idea as the network proxy, now for tools.

## Pin it at boot with `--static-mcp`

```bash
sbx run claude --static-mcp catalog-tools .
```

`--static-mcp` fixes the MCP set at creation. The agent talks to exactly this
server, and the set **can't drift** after the box starts. Your Cedar
`permit(... Mcp::call ... in StaticMcp)` rule from the last section applies on top,
so even a pinned server is only callable if Cedar agrees.

> [!NOTE]
> Two locks, one surface: `--static-mcp` fixes *which* servers exist; Cedar
> decides *whether* a given call is allowed. Neither trusts the agent to behave.

Next: everything the box decided - allowed, denied, brokered - in one log.

# Author a Cedar Authorization Policy

Network and filesystem controls are coarse: *reachable or not*, *writable or not*.
Cedar adds a finer layer - **authorization** over the agent's individual actions.
Every shell command, file write, and MCP tool call is checked against a policy
before it runs.

A starter policy is already in the workspace. Read it:

```bash
cat authz.cedar
```

Three rules, in Cedar's `permit` / `forbid` grammar:

- **permit** shell + file writes - but only inside the workspace `Folder`
- **permit** MCP tool calls - but only to the `StaticMcp` server (next section)
- **forbid** reading anything in `Secrets` (`.env`, `*.pem`, `id_*`) - and a
  `forbid` always beats a `permit`

## Apply it

```bash
sbx policy authz set authz.cedar
```

```bash
sbx policy authz ls
```

## Test it before you trust it

Cedar lets you check a single action without running the agent. A write inside the
workspace is allowed:

```bash
sbx policy authz test src/routes/products.js
```

A read of the secrets file is denied - the `forbid` rule wins:

```bash
sbx policy authz test .env
```

```text no-run-button
Decision: DENY
  action:    File::"read"
  resource:  .env
  policy:    forbid(principal, action, resource) when resource in Secrets;
```

That's the payoff: even though `.env` sits in the writable workspace, the agent
can't read it. The real database password stays out of the model's context.

> [!NOTE]
> Cedar is declarative and testable - the same policy file travels with the box
> (you'll see it in `sbxenv.yaml` at the end), so authorization is reviewed and
> versioned like any other code.

Next: the policy permits tool calls to one MCP server. Let's pin that server.

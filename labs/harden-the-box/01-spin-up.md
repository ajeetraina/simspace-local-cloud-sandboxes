# Spin Up the Box - Local and Cloud

Same box, two places to run it: on your machine, or in Docker Cloud. Start local.

## Local: `sbx run`

```bash
sbx run claude --name catalog .
```

That boots a microVM, mounts the $$app$$ at `~/catalog-service-node`, and drops
you at the agent - with the host filesystem read-only and its own isolated Docker
daemon. Prefer Codex? Same command, different agent:

```bash
sbx run codex --name catalog .
```

Confirm it's up:

```bash
sbx ls
```

## Cloud: `launch.sh`

For a sandbox that outlives your laptop, run it in Docker Cloud. The demo repo's
`launch.sh` is a thin wrapper over `sbx --cloud run`:

```bash
./launch.sh
```

It prints how to store the credential and attach. Under the hood it's the same
sandbox model - just hosted:

```bash
sbx --cloud run claude --name catalog .
```

```bash
sbx --cloud secret set anthropic --oauth
```

```bash
sbx --cloud attach catalog
```

The only difference is `--cloud`. Every hardening step that follows works
identically on both; the credential is brokered by the proxy either way, so the
box never holds the raw value.

> [!NOTE]
> `--name catalog` matters: the policy, Cedar, MCP, and audit commands in the next
> sections all target the box by that name.

Next: decide what the box can reach.

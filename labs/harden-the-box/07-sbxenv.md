# Ship It: sbxenv.yaml for the Catalog

You've hardened one box by hand: policy, filesystem, Cedar, MCP. That's great for
*you* - but a teammate running `sbx run claude` gets none of it. The fix is to
declare the whole box once, in an **`sbxenv.yaml`**, and commit it like code.

One is already in the workspace. Read it:

```bash
cat sbxenv.yaml
```

Every control from this lab is in that one file:

- `policy.network` → the allow/deny rules from **Add a Network Policy**
- `policy.authz: authz.cedar` → the Cedar policy from **Author a Cedar Policy**
- `mcp.static: [catalog-tools]` → the pinned server from **Lock the MCP Surface**
- `secrets.dhi-mcp-pat.ref` → a **source** (`op://...`), not a value: resolved on
  each developer's own host, injected by the proxy, never committed

## Preview, then reproduce

Dry-run it first - `sbx env plan` shows exactly what would be created, with an
approval gate, and changes nothing:

```bash
sbx env plan
```

Then build the whole hardened box from the file:

```bash
sbx env run
```

```text no-run-button
── LOAD ENVIRONMENT  (sbxenv.yaml)
  ✓ network policy applied (allow registries, deny exfil hosts)
  ✓ Cedar authorization loaded (authz.cedar)
  ✓ MCP server catalog-tools registered + authorized (central)
  ✓ secret dhi-mcp-pat resolved from op:// on THIS host (never committed)
── BOOT  Creating sandbox 'catalog'...
  ✓ microVM up; Product Catalog mounted at ~/catalog-service-node
```

That's the whole lab in one command. A teammate clones the repo, runs `sbx env
run`, and gets the byte-for-byte identical box - same policy, same Cedar rules,
same pinned MCP, their own brokered secret. No drift, no "works on my machine."

> [!NOTE]
> This is the handoff from *one secured box* to *a team standard*. The same file
> runs in the cloud - `sbx --cloud env run` - so local and hosted sandboxes stay
> identical too.

You've hardened the box end to end - and made it reproducible.

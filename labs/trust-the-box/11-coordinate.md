# 05 · Co-ordinate

> **At scale.** The same box, running in the cloud — more agents than a laptop can
> hold, every action governed and audited centrally.

This is the "**laptop to cloud**" step. The same `.sbxenv.yaml` that reproduced a
sandbox on your machine runs **remotely**, unchanged:

```bash
sbx cloud run
```

Eight cloud sandboxes, each running the **$$app$$**, in parallel. One laptop could
never hold them; the definition didn't change to get there. List them:

```bash
sbx cloud ls
```

And this is where an **organization** co-ordinates the whole fleet. Contain and
Control are per-sandbox; at scale they're set once and enforced everywhere:

| Developer (local) | Organization (co-ordinated) |
|---|---|
| choose a profile, add rules | **central policy**, enforced on every sandbox |
| your own policy log | **audit log** across the whole fleet |
| pick any kit | **approved agents and kits** only |

```text no-run-button
  Contain + Control  ─────▶  set per sandbox        (the developer)
         │
         ▼
  Co-ordinate        ─────▶  set once, enforced everywhere + audited   (the org)
```

> **Product Catalog mapping.** Ten engineers (and their agents) work the catalog at
> once — some hardening images, some writing tests, some on features. Every box is
> contained the same way, reaches only approved endpoints, runs only approved kits,
> and writes to one central audit log. Security scales with the fleet instead of
> fighting it.

That's the whole arc: one agent, one app, from a risky laptop to a governed cloud.
Continue to the **Conclusion**.

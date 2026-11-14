<!--
layout: section
eyebrow: "05 · Co-ordinate"
-->

# Co-ordinate

The same box, in the cloud — more agents than a laptop can hold, every action
governed and audited centrally.

---

## Live: the same env, in the cloud

The `.sbxenv.yaml` that reproduced a box on your laptop runs **remotely, unchanged**:

```bash terminal-id=demo
sbx cloud run
```

::terminal{id=demo height=280}

Note: This is the "laptop to cloud" moment. Eight cloud sandboxes, each running the
Product Catalog, in parallel. One laptop could never hold them — and the definition
didn't change to get there.

---

<!-- theme: light -->

## Contain and Control are per-sandbox. Co-ordinate is per-org.

| Developer (local) | Organization (co-ordinated) |
|---|---|
| choose a profile, add rules | **central policy**, enforced everywhere |
| your own policy log | **audit log** across the whole fleet |
| pick any kit | **approved agents and kits** only |

```text
  Contain + Control  ──▶  set per sandbox              (the developer)
         │
         ▼
  Co-ordinate        ──▶  set once, enforced + audited (the org)
```

---

<!--
layout: split
columns: 1 1
theme: light
-->

# Product Catalog mapping

<!-- region -->

### The scene

Ten engineers and their agents work the catalog at once — hardening images, writing
tests, shipping features.

<!-- region -->

### The guarantee

Every box contained the same way, reaching only approved endpoints, running only
approved kits, writing to one central audit log.

:::card{accent=green variant=fill}
Security scales **with** the fleet instead of fighting it.
:::

Note: This is the payoff of the whole arc — the thing that was a risky laptop in
Part 1 is now a governed cloud fleet, and the agent never got more trustworthy. The
box did.

<!--
layout: section
eyebrow: "02 · Control"
-->

# Control

Contain is always on. Control is the dial you turn — and every connection is logged.

---

## Live: what did the agent try to reach?

```bash terminal-id=demo
sbx policy log
```

::terminal{id=demo height=300}

Note: Read the bottom two lines out loud. While containerizing the catalog, the agent
reached for production Postgres and S3. Balanced policy denied both — and recorded
the attempt. On the laptop, those would have silently succeeded.

---

<!--
layout: stats
theme: light
-->

## Three profiles, plus your own rules

:::stat{label="most restrictive" value="Locked" accent=blue}
only what you explicitly allow
:::

:::stat{label="default" value="Balanced" accent=green}
common dev endpoints; everything else denied
:::

:::stat{label="least restrictive" value="Open" accent=amber}
outbound allowed, still logged
:::

Add allow/deny rules on top of any profile — they take effect immediately, no restart.

---

<!--
layout: split
columns: 1 1
theme: light
-->

# Product Catalog mapping

<!-- region -->

### Needs

npm · Docker Hub · the model API · the WireMock pricing upstream — to build and test.

<!-- region -->

### Does not need

Your **production database** or **S3 bucket**. Control draws exactly that line, and
the log proves where the agent actually went.

Note: The point is not "lock everything down." It's that you can let the agent work
while drawing a precise, auditable line around prod.

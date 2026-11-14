# 02 · Control

> **Where the boundary sits.** Let the agent work, and decide exactly what it can
> reach — with every connection logged.

Contain is always on. **Control** is the dial you turn. The proxy that injects
credentials also enforces a **network policy**, and it logs every outbound request
— allowed or denied.

## What did the agent try to reach?

```bash
sbx policy log
```

Read the bottom two lines. While containerizing the catalog, the agent reached for
**`prod-db.acme.internal:5432`** and **`s3.amazonaws.com`** — production Postgres
and S3. The *balanced* policy denied both, and recorded the attempt. On the laptop,
those connections would have simply succeeded, silently.

## Three profiles, plus your own rules

```bash
sbx policy ls
```

| Profile | Behavior |
|---|---|
| **Locked Down** | only what you explicitly allow |
| **Balanced** *(default)* | common dev endpoints allowed, everything else denied |
| **Open** | outbound allowed, still logged |

Need a host the default doesn't cover? Add a rule — it takes effect immediately, no
restart:

```bash
sbx policy allow network registry.npmjs.org
```

> **Product Catalog mapping.** The agent legitimately needs npm, Docker Hub, the
> model API, and the WireMock pricing upstream to build and test the catalog. It
> does **not** need your production database or S3 bucket. Control lets you draw
> exactly that line — and the log proves where the agent actually went.

The boundary is set and audited. Now let's make sure the agent has the *tools* it
needs inside it. Continue to **03 · Choice**.

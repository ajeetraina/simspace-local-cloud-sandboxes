<!--
layout: section
eyebrow: "03 · Choice"
-->

# Choice

A locked box is safe. It also has to carry *your* agent, *your* tools, and *your*
endpoints — without weakening the boundary.

---

<!--
layout: split
columns: 1 1
theme: light
-->

# Kits define what runs inside

<!-- region -->

:::card{label="agent kit — one per sandbox" accent=blue}
The harness: which coding agent runs. Claude Code, Codex, Gemini, …
:::

:::card{label="mixin kits — stack as many as you need" accent=green}
Tools, credentials, and rules on top: Node dev env, Postgres client, Docker Scout,
GitHub SSH.
:::

<!-- region -->

```yaml filename=".sbxenv.yaml"
agent: claude
kits:
  - node-dev-kit
  - postgres-client-kit
  - docker-scout-kit
```

The microVM and proxy **don't change**. Only the inside does.

---

## Live: use the Scout kit to fix the base image

Remember `FROM node:20`? Scan the baseline the agent built:

```bash terminal-id=demo
docker scout quickview catalog-service:baseline
```

::terminal{id=demo height=260}

Note: 8 High CVEs — almost none your code. They came in with the base image and its
431 packages. The Scout kit is inside the box, so the agent can find this itself.

---

<!--
layout: split
columns: 1 1
theme: light
-->

# Swap the base, not the app

<!-- region -->

:tag[Before]{accent=red} `node:20` — 431 packages, **8 High**, 1.1GB

:tag[After]{accent=green} hardened, distroless — 78 packages, **0 High**, 248MB

<!-- region -->

```dockerfile filename="Dockerfile.dhi"
FROM dhi.io/node:24-debian13-dev AS build
RUN npm ci --production --ignore-scripts

FROM dhi.io/node:24-debian13   # distroless
COPY --from=build /app/node_modules ./node_modules
```

:::stat{value="8 → 0" label="high cves" accent=green}
by changing the base image, not the app
:::

Note: Product Catalog mapping lives right here: Choice is what makes the box usable
for THIS app — Node, Postgres, Scout inside, and a hardened base — with the boundary
untouched.

# 03 · Choice

> **What's inside.** A locked box is safe. It also has to carry *your* agent, *your*
> tools, and *your* endpoints — without weakening the boundary.

Security tools usually make you trade flexibility for safety. Sandboxes keep the
boundary fixed (microVM + proxy) and make the *inside* configurable, through
**kits**:

- an **agent kit** — which harness runs (Claude Code, Codex, …): one per sandbox,
- **mixin kits** — tools, credentials, and rules stacked on top: as many as you need.

The catalog's sandbox declares exactly that in its `.sbxenv.yaml`:

```bash
cat .sbxenv.yaml
```

A Node dev environment, a Postgres client, and **Docker Scout** — the tools this
app needs — all inside the same unchanged boundary. Let's use the Scout kit to fix
the problem we flagged back in Section 3: the bloated `node:20` base.

## Scan the baseline

```bash
docker scout quickview catalog-service:baseline
```

**8 High** CVEs — almost none of them your code. They came in with the base image
and its 431 packages.

## Harden the base — write a hardened Dockerfile

Multi-stage: a `-dev` image builds, then only the app and its `node_modules` land
in a tiny distroless runtime with no shell and no package manager.

```dockerfile save-as=Dockerfile.dhi
# build stage: has a shell, npm, and build tools
FROM dhi.io/node:24-debian13-dev AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci --production --ignore-scripts

# runtime stage: distroless, non-root, no shell, no npm
FROM dhi.io/node:24-debian13
WORKDIR /app
COPY --from=build /app/node_modules ./node_modules
COPY . .
EXPOSE 3000
CMD ["node", "src/index.js"]
```

```bash
docker build -t catalog-service:dhi -f Dockerfile.dhi .
```

## Measure it

```bash
docker scout quickview catalog-service:dhi
```

From **8 High to 0 High**, and **1.1GB → 248MB** — by changing the base, not the
app.

> **Product Catalog mapping.** Choice is why the sandbox is usable for *this* app:
> the agent gets Node, a Postgres client, and Scout inside the box, and you pick a
> hardened base image — all without touching the microVM or the proxy.

One sandbox is a start. Next: make every teammate's identical. Continue to **04 · Capacity**.

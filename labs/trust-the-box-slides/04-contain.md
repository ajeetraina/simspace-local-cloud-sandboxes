<!--
layout: section
eyebrow: "01 · Contain"
-->

# Contain

The boundary: a microVM, a proxy, and credentials the agent never holds.

---

## Live: the same escape, contained

Byte-for-byte the command that owned the host. Now the socket belongs to the
**sandbox's own** engine:

```bash terminal-id=demo
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock docker:cli docker ps
```

::terminal{id=demo height=280}

Note: We started the box two slides ago, so this runs in sandbox mode. No prod-db,
no payments-api — there's no host here to reach. Same command, different machine.

---

<!-- theme: light -->

## The credential the agent *does* need — injected outside

```text
  SANDBOX          PROXY (outside the box)              INTERNET
  call API    ──▶  destination allowed?           ──▶   api.anthropic.com
  authz: none      inject key from host keychain         200 OK
```

:::card{label="the raw key never enters the box" accent=green variant=fill}
The request leaves with `authorization: (none)`. The proxy adds the credential. The
agent gets the answer and never sees the key — so it can't leak, copy, or bake it
into a layer.
:::

---

<!--
layout: split
columns: 1 1
theme: light
-->

# Product Catalog mapping

<!-- region -->

### On the laptop

Agent read `~/.aws/credentials`, baked `.env` into the image, and listed host
containers through the socket.

<!-- region -->

### In the box

Builds and tests the catalog in its **own** Docker engine. AWS and model keys are
injected by the proxy — they never land in a `.env` the agent can read.

Note: Every C ends on this split: what happened on the laptop, what happens in the
box, for this exact app.

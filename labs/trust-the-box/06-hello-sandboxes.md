# Hello, Sandboxes

A **Docker Sandbox** is a private machine for your agent — a microVM with its own
kernel, its own filesystem, and its own Docker engine, reached only through a proxy
that applies your policy. You give the agent full freedom *inside* a boundary it
can't cross.

```text no-run-button
  YOUR LAPTOP                         THE SANDBOX (microVM)
  ~/.aws, ~/.ssh                      agent — full freedom inside
  other projects         ╳  ──────▶   ./product-catalog  (only this is shared)
  host Docker engine                  its own Docker engine
                                              │
                                         [ proxy ]  ──▶ npm · Docker Hub · model API
                                       the only way out
```

Start it on the **$$app$$** repo — this flips the boundary **on**:

```bash
sbx run claude
```

The same agent is now running inside the box. The goal hasn't changed; the blast
radius has. Three things are now true that weren't a minute ago:

1. **The host is out of reach** — different kernel, different filesystem, a Docker
   engine that isn't yours.
2. **Only the project is shared** — `./product-catalog`, nothing else on the laptop.
3. **Credentials are held outside** — the proxy injects them on the way out; the
   agent never holds the raw key.

> **Trust the box, not the agent.** We stop asking "is this agent safe?" and start
> asking "what can anything inside this box reach?" — a question with a definite
> answer.

Now let's test it. We'll re-run the exact moves from Sections 4 and 5 and watch
them fail to escape. Continue to **01 · Contain**.

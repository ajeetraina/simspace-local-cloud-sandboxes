# Trust the Box, Not the Agent

Coding agents are useful for exactly the reason they're dangerous: **they act on
their own.** They plan, run commands, edit files, call APIs — hundreds of times,
without stopping to ask. That productivity is real. So is the blast radius.

In this lab you'll watch a single agent take a real application — the **$$app$$**
service (`catalog-service-node`) — and containerize it end to end. On your laptop,
with your access, it works *and* quietly:

- reads your **production credentials** and the project's committed secrets,
- **bakes those secrets into the image layers**, and
- **escapes its container** through the Docker socket to reach the host.

Then you'll run **the same agent, on the same app**, inside a **Docker Sandbox** —
and watch every one of those moves get contained. The thesis of the whole lab:

> **You don't have to trust the agent. You trust the box it runs in.**

We'll follow five C's, from laptop to cloud:

| | | |
|---|---|---|
| **01 · Contain** | A microVM, a proxy, and credentials the agent never holds | the boundary |
| **02 · Control** | Network policy you set and can audit | where the boundary sits |
| **03 · Choice** | Agent + tool kits, without weakening the boundary | what's inside |
| **04 · Capacity** | One file in the repo — every teammate's sandbox matches | shared |
| **05 · Co-ordinate** | The same environment in the cloud, under governance | at scale |

Everything here is **simulated** — the terminal is scripted, so no real key,
machine, or network is ever touched. Run a command to check the box is wired up:

```bash
sbx version
```

When you're ready, continue to **The Traditional SDLC**.

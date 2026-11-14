# The Agentic SDLC

Now the same lifecycle, with an agent doing the work. The shape doesn't change —
the *pace* does. The agent runs its own loop:

```text no-run-button
  Plan  →  Act  →  Observe  →  Repeat        (hundreds of times, no human)
   🤖      🤖        🤖          🤖
```

It decides the next step, runs commands, reads the output, and goes again — far
faster than anyone can review. There's no PR to approve between step 40 and step
41. The human isn't in the loop; the human *started* the loop.

Hand the agent a goal and let it work on the **$$app$$**:

```bash
claude "Containerize the Product Catalog and get it running"
```

That one sentence becomes hundreds of tool calls you never see. This is the deal
every developer is now making — and it comes with a fork:

| | SAFE, SLOW | FAST, RISKY |
|---|---|---|
| **Mode** | Approve every action | "YOLO" — skip approvals |
| **Cost** | You become the bottleneck | Agent has *your* access |
| **Feeling** | *"I'm babysitting my agent."* | *"I hope it doesn't touch prod."* |

Most people, most of the time, pick **fast**. So the real question isn't *"should
the agent act on its own?"* — it already does. It's:

> **When the agent runs with my access, what exactly can it reach?**

Let's find out. Continue to **The Agent Containerizes It**.

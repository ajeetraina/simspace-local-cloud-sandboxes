<!--
layout: section
eyebrow: "Part 1"
-->

# The SDLC grew a new developer

And nobody checked its references.

---

<!-- theme: light -->

## The traditional SDLC had a human in every loop

```text
  Plan  →  Code  →  Review  →  Test  →  Merge  →  Deploy
   👤       👤        👤        🤖       👤        👤
```

:::fragment
Slow **by design**. Safe *because* it's slow — permissions, approvals, and change
control all assume a **person** is the one taking the action.
:::

:::fragment
The trust boundary was the person. We trust the developer, so we give the developer
access.
:::

Note: Twenty years of security tooling is built on the assumption that a human is
the actor. Hold that — the next slide breaks it.

---

<!-- theme: light -->

## The agentic SDLC keeps the shape, loses the human

```text
  Plan  →  Act  →  Observe  →  Repeat        (hundreds of times, no human)
   🤖      🤖        🤖          🤖
```

:tag[One sentence in]{accent=neutral} → :tag[hundreds of tool calls out]{accent=amber}

The human didn't stay in the loop. The human **started** the loop and walked away.

Note: There is no PR between step 40 and step 41. The agent decides, acts, reads the
result, and goes again, faster than anyone can review.

---

<!--
layout: split
columns: 1 1
-->

# So developers pick a side

<!-- region -->

:::card{label="safe, slow" accent=blue}
**Approve every action.** The agent stops to ask. You become the bottleneck, and
the productivity you came for evaporates.
:::

<!-- region -->

:::card{label="fast, risky" accent=red}
**YOLO mode.** Skip the approvals. The agent now has exactly the access you have —
to every key, repo, and service on your machine.
:::

Note: Almost everyone picks fast. So the real question is not "should the agent act
alone" — it already does. It's "what can it reach when it does?"

---

<!--
layout: stats
theme: light
-->

## The agent runs as you

:::stat{value="100%" accent=red}
of your access is the agent's access in YOLO mode
:::

:::stat{value="0" accent=amber}
approvals between the goal and production
:::

:::stat{value="1" label="file" accent=blue}
`.env` the agent reads first — and it's full of secrets
:::

Note: Transition line: let's watch what "runs as you" actually costs, on a real app.

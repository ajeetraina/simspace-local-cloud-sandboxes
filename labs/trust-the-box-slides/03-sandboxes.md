<!--
layout: section
eyebrow: "Part 2 · the answer"
-->

# Hello, Sandboxes

Give the agent everything it needs, inside a boundary it can't cross.

---

<!-- theme: light -->

## A sandbox is a private machine for your agent

```text
  YOUR LAPTOP                          THE SANDBOX (microVM)
  ~/.aws · ~/.ssh                      agent — full freedom inside
  other projects          ╳  ───────▶  ./product-catalog  (only this is shared)
  host Docker engine                   its own Docker engine
                                              │
                                        [ proxy ] ──▶ npm · Docker Hub · model API
                                       the only way out
```

:::fragment
A **microVM** with its own kernel and Docker engine. A **proxy** that's the only way
out. Credentials **injected** on the way out — never held inside.
:::

---

## Live: start the box

Same agent, same goal — new blast radius.

```bash terminal-id=demo
sbx run claude
```

::terminal{id=demo height=300}

Note: Click Run. microVM booted, proxy up, only the project mounted, credentials
armed. From here on, every demo in the talk runs inside this box.

---

<!--
layout: quote
theme: dark
-->

> Stop asking "is this agent safe?" Start asking "what can anything inside this box
> reach?"

**Trust the box, not the agent**

Note: This is the thesis slide — the title of the talk, earned. The second question
has a definite answer; the first one never will.

---

<!--
layout: section
eyebrow: "The five C's"
theme: tint
-->

# From laptop to cloud, in five C's

Contain · Control · Choice · Capacity · Co-ordinate

---

<!-- theme: light -->

## Five C's, mapped to the Product Catalog

| C | The boundary | For the Product Catalog |
|---|---|---|
| **Contain** | microVM + proxy + credential injection | keys injected, never in a readable `.env` |
| **Control** | network policy, fully logged | reaches npm & the model API, **not** prod DB |
| **Choice** | agent + tool kits | Node, Postgres, Scout — boundary unchanged |
| **Capacity** | one file in the repo | every teammate's box matches |
| **Co-ordinate** | cloud + org governance | many agents, one audit log |

Note: This is the map for the rest of the talk. Each C gets one short section and
one concrete thing it does for this specific app.

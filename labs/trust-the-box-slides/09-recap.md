<!--
layout: section
eyebrow: "Recap"
theme: tint
-->

# One agent, one app, two blast radii

The agent never got more trustworthy. The box did.

---

<!-- theme: light -->

## The five C's, earned

| | | |
|---|---|---|
| **Contain** | microVM + proxy + injected credentials | the boundary |
| **Control** | network policy, fully logged | where it sits |
| **Choice** | agent + tool kits, boundary unchanged | what's inside |
| **Capacity** | one `.sbxenv.yaml` in the repo | shared |
| **Co-ordinate** | cloud + central governance | at scale |

:::fragment
On the laptop the agent read prod keys, baked secrets into the image, and escaped the
container. In the box, the same agent did the same work — contained.
:::

---

<!--
layout: quote
theme: dark
-->

> Trust the box, not the agent.

**You decide what anything inside it can do — from your laptop to the cloud**

Note: Land the title one last time. The whole talk in six words.

---

<!--
layout: title
theme: dark
byline: "Now go run your agents — inside a box you trust"
-->

# Get started

**Docs** — $$docs$$ · **Install** — `brew install docker/tap/sbx` · **Then** —
`sbx login`

**Try the lab next:** take the Product Catalog through all five C's yourself — every
command simulated, nothing to install.

Note: Point people at the lab that pairs with this deck — it's the other card on the
landing page. Thank the room.

<!--
layout: title
byline: "Sandboxing coding agents from laptop to cloud"
-->

# Trust the Box, Not the Agent

Your coding agent is going to act on its own. The question isn't whether you trust
it — it's what it can reach when you don't.

Note: Welcome. One sentence to set the frame: we are going to stop trying to decide
whether an agent is trustworthy, and start deciding what the box it runs in will
allow. Keep this slide up while you introduce yourself.

---

<!-- layout: quote -->

> I'm not reviewing my agent's work anymore. I'm babysitting it.

**Every developer, 2026**

Note: This is the line that gets the knowing laugh. The tension of the whole talk:
we want the productivity, but "approve every step" makes us the bottleneck, and
"skip every step" gives the agent our keys.

---

<!--
layout: section
eyebrow: "The shape of the talk"
-->

# One agent, one app, two blast radii

We containerize the Product Catalog on a laptop, then inside a box — and follow
five C's from laptop to cloud.

---

<!-- theme: light -->

## Where we're going

:::card{label="the problem" accent=red}
A coding agent containerizes the **$$app$$** — and reads credentials, bakes secrets
into the image, and escapes its container. No attack required.
:::

:::card{label="the answer" accent=blue}
The same agent inside a **Docker Sandbox** — contained, controlled, and reproducible
from your laptop to the cloud.
:::

The five C's: **Contain · Control · Choice · Capacity · Co-ordinate**.

Note: Agenda without calling it an agenda. The red card is the first half, the blue
card is the second half, and the five C's are the spine of the back half.

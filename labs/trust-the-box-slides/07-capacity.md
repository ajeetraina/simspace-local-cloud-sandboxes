<!--
layout: section
eyebrow: "04 · Capacity"
-->

# Capacity

One developer with one hand-built sandbox is a demo. A team needs the same box
everywhere — reproducibly.

---

<!-- theme: light -->

## Scaling up starts with reproducibility

```text
  One dev, by hand    →    A team, by hand       →    A team, from one file
  works on one box         every setup drifts         every sandbox matches
```

:::fragment
Everything you configured — agent, kits, policy, resources — is **one file committed
to the repo**.
:::

---

## Live: reproduce the whole box from one file

```bash terminal-id=demo
sbx env run
```

::terminal{id=demo height=280}

Note: No README to follow, nothing to install by hand. The agent, the kits, the
policy, the ports — all resolved from .sbxenv.yaml. Your box and your teammate's box
are identical because they come from the same source of truth.

---

<!-- theme: light -->

## It changes like code

| 1. Edit the file | 2. Open a PR | 3. Merge | 4. Everyone pulls |
|---|---|---|---|
| add a kit, swap the agent | reviewed like code | repo holds the definition | next box matches |

:::card{label="product catalog mapping" accent=blue variant=fill}
The catalog's `.sbxenv.yaml` sits next to its `compose.yaml`. A new teammate clones
the repo, runs `sbx env run`, and gets the same contained, controlled, fully-equipped
box — on the first try.
:::

Note: Capacity is the quiet one, but it's what turns a cool demo into a team practice.
Update once, everyone follows.

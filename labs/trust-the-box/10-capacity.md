# 04 · Capacity

> **Shared.** One developer with one hand-built sandbox is a demo. A team needs the
> same sandbox everywhere — reproducibly.

Everything you've configured so far — the agent, the kits, the network policy, the
resources — lives in **one file committed to the repo**: `.sbxenv.yaml`. You already
read it in the last section. It *is* the sandbox definition.

```bash
cat .sbxenv.yaml
```

Because it's in the repo, anyone who clones the **$$app$$** can recreate the exact
box with one command — no README to follow, nothing to install by hand:

```bash
sbx env run
```

The agent, the Node/Postgres/Scout kits, the balanced policy, the ports — all
resolved from the file. Your sandbox and your teammate's sandbox are identical
because they come from the same source of truth.

```text no-run-button
  One dev, by hand   →   A team, by hand        →   A team, from one file
  works on one box       every setup drifts         every sandbox matches
```

And it changes like code:

| 1. Edit the file | 2. Open a PR | 3. Merge | 4. Everyone pulls |
|---|---|---|---|
| add a kit, swap the agent | reviewed like code | repo holds the new definition | next sandbox matches |

> **Product Catalog mapping.** The catalog's `.sbxenv.yaml` is committed next to its
> `compose.yaml`. A new teammate clones the repo, runs `sbx env run`, and gets the
> same contained, policy-controlled, fully-equipped box you've been using — on the
> first try.

One laptop can only hold so many boxes, though. The last C takes this to the cloud.
Continue to **05 · Co-ordinate**.

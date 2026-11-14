<!--
layout: title
byline: "One coding agent, one real job. Only the box around it changes."
-->

# Trust the box, not the agent

Hero app: **`dockersamples/catalog-service-node`** — a real Product Catalog we hand
to an agent, then change only the box around it.

Note: Open here. The whole talk is one agent, one job, run twice — once loose on the
host, once in a box. The agent never gets more trustworthy. The box does.

---

<!-- theme: light -->

## 30 minutes, four C's

:::card{label="the problem" accent=red}
One prompt, seven problems. An agent containerizes the Product Catalog on a laptop
and wrecks every security review.
:::

:::card{label="the answer" accent=blue}
**Contain · Control · Choice · Capacity** — one pillar per problem, each with a live
demo. Same agent, same prompt, a different box.
:::

Then: the scoreboard, and the labs you'll run yourself.

Note: Four parts. ~8 min on the problem, ~18 on the four C's and their demos, ~4 to
land it. The labs are the hands-on half.

---

<!--
layout: stats
theme: light
-->

## Every useful agent has all three

:::stat{label="01 access" value="Private data" accent=red}
code, credentials, customer records
:::

:::stat{label="02 exposure" value="Untrusted input" accent=amber}
web pages, emails, tool output
:::

:::stat{label="03 action" value="External reach" accent=red}
send, call APIs, hit the internet

:::

:::fragment
You can't **train** it out, **prompt** it out, or **policy-document** it out. One
place is left: **enforce at runtime, outside the agent.**
:::

Note: The lethal trifecta. Every agent worth using has all three by design — that's
why the fix has to be a boundary at runtime, not a better prompt.

---

<!--
layout: split
columns: 1 1
theme: light
-->

# Meet the Product Catalog

<!-- region -->

A real Node.js service: **Postgres**, **Kafka**, **S3 (LocalStack)**, an upstream
**inventory API**. Ships a Dockerfile and compose.

`$$repo$$`

<!-- region -->

:::card{label="in dev, on a laptop" accent=red variant=fill}
**Plaintext in `.env`, next to the code:**
DB password · AWS keys · inventory API key · Stripe key
:::

Note: The app is realistic enough to be dangerous: to do its job it needs a DB
password, cloud keys, an inventory key, a Docker engine, and network. That's the
whole "what weakens isolation" list, forced on stage by one app.

---

<!-- theme: light -->

## The traditional SDLC: a human at every gate

```text no-run-button
  Plan ─▶ Code ─▶ Build ─▶ Test ─▶ Secure ─▶ Ship
   👤      👤       👤       🤖      👤        👤
  ticket  Docker   Build   Test-   scan +    registry
          files    Cloud   cont.   review    deploy
```

Speed was limited by humans. **So was the blast radius.**

:::fragment
*"Why is there an AWS key in this Dockerfile?"* — a reviewer, catching it before it ships.
:::

Note: Twenty years of security assumes a person is the actor at each gate. A human is
slow — and that slowness is what caught the bad decision.

---

<!-- theme: light -->

## The agentic SDLC: plan, act, observe, repeat

```text no-run-button
  Plan ─▶ Act ─▶ Observe ─▶ Repeat        the same six gates,
   🤖     🤖       🤖          🤖           now with no person at any of them
```

Minutes to finish. **No person in the loop.** The gates are still there — the human
standing at each one is gone.

Note: Same six stages, same app. The only thing removed is the person. Everything
the person used to catch now ships.

---

<!--
layout: default
theme: dark
-->

## The prompt

```text no-run-button
$ claude "Containerize the Product Catalog, make the integration
          tests pass, and push an image. Don't ask me questions,
          I'm in a meeting."
```

:tag[YOLO mode]{accent=red} → runs on the host → no human gate

Note: This is the prompt, run the way people really run it — YOLO, on the host, "don't
ask me." The tests will go green. It will also fail every review the old SDLC had.

---

<!-- theme: light -->

## What the agent did: seven steps, tests green

| # | Step | What it exposed |
|---|---|---|
| 1 | `FROM node:20` | 431 packages, 47 CVEs |
| 2 | `COPY . .` | `.env` keys baked into an image layer |
| 3 | `cat ~/.aws/credentials` | real cloud keys in agent context |
| 4 | `-v /var/run/docker.sock` | full control of the host Docker |
| 5 | `npm install lookalike-pkg` | install script posts env vars out |
| 6 | `POSTGRES_PASSWORD=postgres` | known password in a public image |
| 7 | 6 teammates, 6 setups | six different risk profiles |

Note: Seven real steps, each reasonable in isolation, tests green throughout. The row
numbers carry through to the scoreboard at the end — remember them.

---

<!--
layout: stats
theme: light
-->

## Three problems, seven rows

:::stat{label="rows 1 · 6" value="Vulnerable images" accent=red}
picks what works, not what's safe
:::

:::stat{label="rows 2 · 3" value="Exposed keys" accent=red}
reads whatever you can read
:::

:::stat{label="rows 4 · 5 · 7" value="Too much reach" accent=red}
reaches whatever your laptop reaches
:::

> The agent didn't go rogue. It had **our keys, our socket, and our network.**

Note: Group the seven into three. This is the whole problem statement — and each of
the four C's will close a specific group.

---

<!--
layout: quote
theme: dark
-->

> 25,000 documents deleted from the wrong database. 13 years of photos gone, Trash
> bypassed. Production data wiped — and it was technically allowed.

**Refuse the choice between "approve every action" and "YOLO mode."**

Note: These are real incidents. The usual answer is approve-everything (you're the
bottleneck) or YOLO (your access, none of your judgment). We refuse both: fast AND
safe, no babysitting.

---

<!--
layout: split
columns: 1 1
theme: light
-->

# Why not containers?

<!-- region -->

Each capability you grant the agent punches a hole in the wall:

:::card{label="the dashed lines" accent=amber}
credentials · host Docker socket · network · host files (`~/.aws` next door)
:::

<!-- region -->

| Give it | Isolation left |
|---|---|
| `-v $PWD:/work` | host files writable |
| `--network host` | any endpoint |
| `-e KEY=…` | the raw credential |
| `-v docker.sock` | the host's engine |

Note: A container is a fine boundary for an application. An agent needs credentials, a
Docker engine, and the network — and each one weakens the container until it isn't one.

---

<!--
layout: split
columns: 1 1
theme: light
-->

# Hello, Sandboxes

<!-- region -->

### Your laptop (host)

`~/.aws/credentials` · host Docker socket · real API keys · every other folder —
**never enters the VM.**

<!-- region -->

### `sbx run claude` · microVM

The agent (full sudo, own kernel) + `catalog-service-node/` (the only mount) + its
own Docker engine. The one way out is a **host proxy** that holds policy, keys, and
the audit log.

Note: A sandbox is a private machine for the agent: its own kernel and Docker engine,
only the project mounted, and a proxy as the single exit. Full freedom inside, a hard
wall at the edge.

---

<!--
layout: split
columns: 1 1
theme: light
-->

# Container vs sandbox: whose kernel?

<!-- region -->

:tag[Container]{accent=amber} shares the **host kernel**. An escape targets *your*
kernel. Fine for apps; thin for an agent.

<!-- region -->

:tag[Sandbox]{accent=green} is a **microVM** with its own kernel behind a hypervisor.
An escape must cross the hypervisor. `sudo` inside is root of a *disposable* machine.

:::fragment
We took the kernel out of the equation.
:::

Note: The core difference. Sudo inside a sandbox is fine because it's root of a throwaway
VM, not your laptop. The hypervisor is the control, not in-VM permissions.

---

<!-- theme: light -->

## The 4 C's: one pillar per problem

| | Pillar | What it does | Closes |
|---|---|---|---|
| **01** | Contain | can't take what it can't see | rows 2 · 3 · 4 |
| **02** | Control | decide where the walls sit | row 5, starts 6 |
| **03** | Choice | the right tools inside the box | rows 1 · 6 |
| **04** | Capacity | one box for the whole team | row 7 |

> **Same agent. Same prompt. Only the box changes.**

Note: The map for the rest of the talk. Four pillars, each closes specific rows from
the scoreboard. Each gets one explainer and one live demo.

---

<!--
layout: section
eyebrow: "01 · Contain"
-->

# Contain

The agent can't take what it can't see. MicroVM, own Docker engine, keys injected by
the proxy — the tests still pass, but the keys were never in the room. *Closes rows 2 · 3 · 4.*

Note: Contain is the boundary itself. Next slide is the live demo — flip it between "no
sandbox" and "in a sandbox" and watch the same tasks succeed or bonk off the wall.

---

<!-- chrome: false -->

<iframe src="labs/trust-the-box-slides/assets/the-box.html" title="The Box — what the agent can reach, with and without a sandbox" style="position:absolute;inset:0;width:100%;height:100%;border:0;background:#fff"></iframe>

Note: DEMO. Start on "No sandbox" — ask it to find passwords, free up disk: it walks
right in. Flip to "In a sandbox": only the project came in with it; everything else
bonks off the wall. The key never enters the VM, so `COPY . .` has nothing to bake.

---

<!--
layout: section
eyebrow: "02 · Control"
-->

# Control

One door, one guard, every attempt recorded. Developers pick a profile; the
**organization** writes one policy in Docker Home — default-deny, enforced on every
sandbox, with an audit log. A local allow rule goes **inactive** under org policy.
*Closes row 5, starts row 6.*

Note: Control decides where the wall sits. This one slide folds in org governance: the
admin's policy wins, blocked requests say "blocked by org policy," and security sees
the attempt unasked. Demo next.

---

<!-- chrome: false -->

<iframe src="labs/trust-the-box-slides/assets/the-door.html" title="The Door — the guard, the allow-list, and the policy log" style="position:absolute;inset:0;width:100%;height:100%;border:0;background:#fff"></iframe>

Note: DEMO. Send messages to the AI model and GitHub (allowed) and to the news/sketchy
hosts (denied). Watch the guard's allow-list and the live `sbx policy log` fill with
ALLOW/DENY — the exfil host from row 5 is blocked and recorded.

---

<!--
layout: section
eyebrow: "03 · Choice"
-->

# Choice

A locked box is safe — but empty. Add exactly the tools the job needs: the agent kit,
mixins, and a **hardened base**. `FROM node:20` (47 CVEs) → `dhi.io/node` (0 CVEs),
signed, SBOM, non-root. The microVM and proxy never move. *Closes rows 1 · 6.*

Note: Choice is what makes the box usable. Same walls, better tools inside. The deny
rule for third-party registries is the guarantee; the DHI steering is the ask.

---

<!-- chrome: false -->

<iframe src="labs/trust-the-box-slides/assets/the-kit.html" title="The Kit — add kits to the box and harden the base image" style="position:absolute;inset:0;width:100%;height:100%;border:0;background:#fff"></iframe>

Note: DEMO. Dock the agent kit and mixins into the box, then "harden the base" — watch
8 High CVEs drop to 0 while the boundary stays exactly where it was.

---

<!--
layout: section
eyebrow: "04 · Capacity"
-->

# Capacity

One `sbxenv.yaml` in the repo describes the whole box — agent, kits, resources. Every
teammate runs `sbx env run` and gets the *identical* sandbox; a kit change is a pull
request. Same file runs on **Cloud Sandboxes** — fan agents out in parallel.
*Closes row 7.*

Note: Capacity turns one good box into the team's box. Commit the file; everyone
follows. Then the same file runs in the cloud — one agent hardens the Dockerfile,
another writes tests, another updates Kafka, all at once.

---

<!-- chrome: false -->

<iframe src="labs/trust-the-box-slides/assets/the-file.html" title="The File — one sbxenv.yaml, four identical boxes" style="position:absolute;inset:0;width:100%;height:100%;border:0;background:#fff"></iframe>

Note: DEMO. "By hand": four laptops drift into four different setups. Flip to "From the
file": one sbxenv.yaml, four identical boxes. Update once, every sandbox follows — on
any machine, laptop or cloud.

---

<!-- theme: light -->

## Scoreboard: every row closed

| # | What went wrong | Problem | Closed by |
|---|---|---|---|
| 1 | `FROM node:20` | images | **Choice** |
| 6 | hard-coded Postgres password | images | **Choice** |
| 2 | `.env` baked into a layer | keys | **Contain** |
| 3 | `~/.aws/credentials` read | keys | **Contain** |
| 4 | `docker.sock` mounted | reach | **Contain** |
| 5 | lookalike package, exfil host | reach | **Control** |
| 7 | six laptops, six setups | reach | **Capacity** |

Note: Seven rows, four pillars, every one closed — and we never made the agent more
trustworthy. Point at each row and name its C.

---

<!--
layout: split
columns: 1 1
theme: light
-->

# Before and after, same prompt

<!-- region -->

### Act 2 · agent on the host

node:20 — 431 pkgs, 47 CVEs · keys in the layer · host socket · reaches anything ·
different on every laptop · **no evidence**

<!-- region -->

### Act 4 · governed sandbox

`dhi.io/node` — 211 pkgs, 0 CVEs · keys injected, never in the VM · its own engine ·
org allowlist, logged · one `sbxenv.yaml` · **audit log of every decision**

Note: Same agent, same prompt, a different box. Read the two columns straight down —
every line flips from liability to evidence.

---

<!--
layout: title
theme: dark
byline: "Run the lab · sandboxing.dockerworkshop.com · CONTAIN · CONTROL · CHOICE · CAPACITY"
-->

# Trust the box, not the agent

We never made the agent more trustworthy. We made the box worth trusting.

Note: Land the title one last time. Point them at the labs that pair with this deck —
the same Product Catalog, the same four C's, hands-on. Thank the room.

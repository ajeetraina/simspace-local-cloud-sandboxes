# Trust the Box, Not the Agent

> **Sandboxing coding agents from laptop to cloud.**

A hands-on [Simspace](https://github.com/dockersamples/simspace) workshop — a
conference **slide deck** and a paired **hands-on lab**, both running entirely in
the browser with a simulated terminal. Nothing to install; no real machine, key,
or network is ever touched.

## The story

A single coding agent is told to *"containerize the Product Catalog"* (the
[`catalog-service-node`](https://github.com/dockersamples/catalog-service-node)
sample app). On a trusted laptop it succeeds — **and** reads your production
credentials, bakes secrets into the image layers, and escapes its container
through the Docker socket. None of it is an attack; it's a capable agent running
with your access.

Then the same agent runs the same job inside a **Docker Sandbox**, and every one of
those moves gets contained. The workshop follows five C's, from laptop to cloud:

| | | Mapped to the Product Catalog |
|---|---|---|
| **01 · Contain** | microVM + proxy + credential injection | keys injected by the proxy, never in a readable `.env` |
| **02 · Control** | network policy, fully logged | reaches npm & the model API, **not** prod DB or S3 |
| **03 · Choice** | agent + tool kits | Node, Postgres, Docker Scout inside — boundary unchanged |
| **04 · Capacity** | one `.sbxenv.yaml` in the repo | every teammate's sandbox matches |
| **05 · Co-ordinate** | cloud + org governance | many agents, one central audit log |

> **Trust the box, not the agent.** You don't predict what the agent will do — you
> decide what anything inside the box *can* do, from your laptop to the cloud.

## What's here

```
labs/
  trust-the-box-slides/   # the conference deck (kind: slides) — "watch the talk"
    labspace.yaml         # kind: slides, theme, brand, reuses the lab's simulator
    00-opening.md … 09-recap.md   # one chapter per file, slides split on `---`
  trust-the-box/          # the hands-on lab — "now you try it"
    labspace.yaml         # title, catalog card, terminal, seed files, sections
    simulator.yaml        # every command's behaviour (scenarios); shared by the deck
    00-introduction.md … 12-conclusion.md   # one file per section
```

The deck's live demos reuse the lab's `simulator.yaml`, so what you demo on stage is
exactly what learners run themselves — the two can't drift apart.

## Author / preview locally

You only need Docker.

```bash
docker compose up dev              # live preview at http://localhost:5173
docker compose run --rm validate   # validate both entries (fails on errors)
```

Edit files under `labs/<id>/` and refresh the browser. The `labs.json` catalog is
**generated** — never edit it by hand.

- **Present the deck:** open the `trust-the-box-slides` card → `p` for present mode,
  `f` for fullscreen, `s` for speaker notes, arrow keys to advance (fragments
  included). Click **Run** on a demo slide to fire the terminal.
- **Take the lab:** open the `trust-the-box` card and work left-to-right; each
  hands-on section has a milestone that completes when you run its command.

## Deploy

**GitHub Pages (default):** enable Pages (Settings → Pages → Source: "GitHub
Actions") and push to `main` — `.github/workflows/deploy.yml` validates and
publishes. PRs are validated by `.github/workflows/validate.yml`.

**As a container:**

```bash
docker build -t trust-the-box .
docker run --rm -p 8080:80 trust-the-box    # http://localhost:8080
```

## Author with an AI agent

[`.sbxenv.yaml`](.sbxenv.yaml) describes a Docker Sandbox for authoring this repo —
fittingly, the same technology the workshop teaches:

```bash
sbx env run
```

You get Claude Code with the **authoring-lab** and **authoring-slidedeck** skills
installed, Docker inside the sandbox for `docker compose up dev`, and ports 5173 /
8888 on your own browser.

## Learn more

- **Docker Sandboxes docs** — https://docs.docker.com/ai/sandboxes/
- **Install** — `brew install docker/tap/sbx`, then `sbx login`
- **The Simspace format** — [`dockersamples/simspace`](https://github.com/dockersamples/simspace)
  (`spec/labspace.md`, `spec/simulator.md`, `spec/slidedeck.md`)
- **The sample app** — [`dockersamples/catalog-service-node`](https://github.com/dockersamples/catalog-service-node)

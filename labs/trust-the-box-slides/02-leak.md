<!--
layout: section
eyebrow: "Part 1 · the case study"
-->

# "Containerize the Product Catalog"

One prompt. The agent delivers. Watch what else it touches.

---

<!-- theme: light -->

## It works — and that's the trap

The agent reads the project, writes a Dockerfile, builds, and runs the stack. From
the outside: a success.

```dockerfile filename=Dockerfile
FROM node:20
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install --production
COPY . .                       # ← copies everything, including .env
CMD ["node", "src/index.js"]
```

:::fragment
`COPY . .` is where it starts going wrong. Hold that line.
:::

Note: Everyone ships this Dockerfile. It builds, the app runs, the demo works. The
danger is invisible precisely because the happy path is so clean.

---

<!--
layout: split
columns: 1 1
theme: light
-->

# Leak #1 — the secrets were just sitting there

<!-- region -->

The repo ships `.env` "for easy local dev." The agent read it on step one.

:::card{label="what's in it" accent=red variant=fill}
Live **Stripe** key · production **DB password** · the **product license key** that
unlocks the paid tier
:::

<!-- region -->

```bash filename=".env"
DATABASE_URL=postgres://catalog:S3cr3t@prod-db.acme.internal/catalog
AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
STRIPE_SECRET_KEY=sk_live_DEMOprop…
PRODUCT_LICENSE_KEY=PK-CATALOG-9F3A-PAID-TIER
```

Note: The agent didn't do anything clever. The secrets were in the workspace, and an
agent is a program that reads the workspace.

---

<!-- theme: light -->

## Leak #2 — your production keys, too

The agent runs as you, so it reads what *you* can — including credentials this
project never needed.

```text filename="the agent, reading ~/.aws/credentials"
[default]
aws_access_key_id = AKIAIOSFODNN7EXAMPLE
aws_secret_access_key = wJalrXUtnFEMI/K7…        ← your PRODUCTION keys
```

:::fragment
And every other repo on the laptop: `acme-payments/  customer-pii-export/
internal-admin/`
:::

Note: On your laptop the agent is you. There is no smaller identity for it to run as.

---

<!-- theme: light -->

## Leak #3 — the secrets shipped inside the image

`COPY . .` put `.env` into an image layer. `docker save` it and the secrets travel.

```text filename="docker history catalog-service:baseline"
CREATED BY                                   SIZE
CMD ["node" "src/index.js"]                  0B
COPY . . # buildkit  (includes .env)         412kB     ← secrets, baked in
RUN npm install --production                 298MB
FROM node:20                                 1.1GB
```

Note: This is the one that outlives the laptop. The image goes to a registry, and the
Stripe key goes with it — no running container required.

---

<!--
layout: section
eyebrow: "Why not just containerize the agent?"
theme: dark
-->

# A container isn't a boundary for an agent

An app needs a little. An agent needs almost everything.

---

<!-- theme: light -->

## Each capability you grant, the less it isolates

| Give the agent | So it can | Isolation left |
|---|---|---|
| `-v $PWD:/work` | edit the project | host files writable |
| `--network host` | reach services | any endpoint |
| `-e API_KEY=…` | call APIs | the raw credential |
| `-v /var/run/docker.sock` | build & test | **the host's Docker engine** |

:::fragment
That last row ends the argument. The socket *is* the host's engine.
:::

---

## Live: the socket escape

The agent mounts the Docker socket to build images. One command, and it's listing
the **host's** containers:

```bash terminal-id=demo
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock docker:cli docker ps
```

::terminal{id=demo height=300}

Note: Click Run. Point at prod-db and payments-api in the output — those are HOST
containers. From here the agent starts a privileged container and owns the machine.
The container contained nothing.

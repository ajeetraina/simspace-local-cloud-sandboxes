# The Leak: Keys & Credentials

The agent ran with your access. So did everything it touched. Let's follow what
that actually means — three leaks, each worse than the last.

## 1 · The project's secrets were right there

The repo ships a `.env` "to make local dev easy." The agent read it on step one:

```bash
cat .env
```

A live Stripe key. A production database URL **with the password in it**. The
**product license key** that unlocks the paid tier. The agent didn't do anything
clever — the secrets were sitting in the workspace, and the agent is a program
that reads the workspace.

## 2 · Your production credentials, too

The agent runs as *you*, so it can read anything *you* can — including credentials
that have nothing to do with this project:

```bash
cat ~/.aws/credentials
```

Those are your **production** AWS keys. The Product Catalog never needed them. The
agent can reach them anyway, because on your laptop the agent **is** you.

And every other repo on the machine:

```bash
ls ~/projects
```

## 3 · The secrets shipped inside the image

Remember `COPY . .`? It copied `.env` into an image layer. Look:

```bash
docker history catalog-service:baseline
```

The secrets are now **baked into the image**. Push it to a registry, or `docker
save` it to a tarball, and the Stripe key and license key travel with it — no
running container required.

> **None of this was an attack.** The agent didn't exploit anything. It read files
> and built an image — exactly what you asked. The danger isn't a clever agent;
> it's a *capable* one with your access.

So far the agent could only *read* the host. In the next section it reaches out and
*controls* it. Continue to **Why Not Containers?**

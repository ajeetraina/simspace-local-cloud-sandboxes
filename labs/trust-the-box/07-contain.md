# 01 · Contain

> **The boundary.** A microVM, a network proxy, and credentials the agent never
> holds.

The agent is inside the box now. Let's try the exact attacks that worked on the
laptop and watch them hit the wall.

## The socket escape — contained

Same command as Section 5, byte for byte:

```bash
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock docker:cli docker ps
```

This time the socket belongs to the **sandbox's own** Docker engine — not the
host's. There is no `prod-db`, no `payments-api`, because there is no host here to
reach. The escape route is gone.

## The host keys — gone

```bash
ls ~/.ssh
```

```bash
cat ~/.aws/credentials
```

Empty, and *no such file*. The host's `~/.ssh` and `~/.aws` were never mounted.
The agent can't leak what was never in the box.

## The credential the agent *does* need — injected, never held

The agent still has to call the model API. Watch how:

```bash
curl https://api.anthropic.com/v1/messages
```

The request leaves the sandbox with **`authorization: (none)`**. The proxy — which
sits *outside* the box — checks the destination, then injects the key from the host
keychain on the way out. The agent gets its `200 OK` and never sees the key.

> **Product Catalog mapping.** The agent still builds and tests the catalog in its
> own Docker engine, and still calls the APIs it needs. But the AWS and model keys
> are injected by the proxy — they never land in a `.env` or an environment
> variable the agent can read, copy, or bake into a layer.

The box contains. Next: *you* decide where its edges sit. Continue to **02 · Control**.

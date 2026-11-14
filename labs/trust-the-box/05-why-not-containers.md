# Why Not Containers?

Fair question: the agent already runs Docker — why not just put the *agent* in a
container? Containers isolate applications well. But an agent isn't an application.
An application needs a little. **An agent needs almost everything** — your source,
the network, your keys, and a Docker engine to build and test with.

Watch what each capability costs you:

| Give the agent | So it can | Isolation left |
|---|---|---|
| `-v $PWD:/work` | edit the project | host files now writable |
| `--network host` | reach services | any endpoint, internal or external |
| `-e API_KEY=…` | call APIs | the raw credential, in its environment |
| `-v /var/run/docker.sock` | build & test | **the host's Docker engine** |

That last row is the one that ends the argument. To let the agent build images,
you mount the Docker socket. But the socket *is* the host's Docker engine. Run one
command that uses it:

```bash
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock docker:cli docker ps
```

The agent — from inside its container — just listed the **host's** containers:
`prod-db`, `payments-api`, `admin-console`. From here it can start a new container
that mounts the host's root filesystem, and it owns the machine. **The container
didn't contain anything.**

> The more useful you make the agent, the less the container isolates it. A
> container is the wrong boundary for something that needs this much access.

We need a boundary that gives the agent everything it needs *inside*, while the
host stays out of reach. That's a sandbox. Continue to **Hello, Sandboxes**.

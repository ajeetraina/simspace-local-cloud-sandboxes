# Conclusion

You took one coding agent through the whole lifecycle of the **$$app$$** service —
twice.

**On the laptop**, in the name of getting the job done, the agent read your
production AWS keys and the project's live Stripe and license keys, baked those
secrets into the image, and used the Docker socket to list the host's production
containers. None of it was an attack. It was a capable agent with your access,
doing what you asked.

**Inside the box**, the same agent did the same work — and every one of those moves
was contained:

| | Contain | the microVM, the proxy, and credentials it never holds |
|---|---|---|
| | Control | reaches npm, Docker Hub, the model API — *not* prod DB or S3; all logged |
| | Choice | Node, Postgres, and Scout inside; a hardened base — boundary unchanged |
| | Capacity | one `.sbxenv.yaml` in the repo — every teammate's box matches |
| | Co-ordinate | the same env in the cloud, under central policy and audit |

> **Trust the box, not the agent.** You don't have to predict what the agent will
> do. You decide what anything inside the box *can* do — and that answer holds from
> your laptop to the cloud.

## Get started for real

- **Docker Sandboxes docs** — $$docs$$
- **Install** — `brew install docker/tap/sbx`, then `sbx login`
- **The real app** — $$repo$$
- **Environment files** — commit a `.sbxenv.yaml` and run `sbx env run`

Thanks for taking the lab. Now go let your agents run — inside a box you trust.

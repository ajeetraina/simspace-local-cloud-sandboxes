# One Box, End to End

You're about to hand the **$$app$$** service to a coding agent and let it work -
but inside a box you control. This lab builds that box one control at a time, the
same order you'd harden it for real:

1. **Spin it up** - locally with `sbx run`, and in the cloud with `launch.sh`.
2. **Add a network policy** - decide what the box can reach.
3. **Pin the filesystem** - decide what the box can see and write.
4. **Author a Cedar policy** - decide what actions the agent may take.
5. **Lock the MCP surface** - fix the tool servers at boot.
6. **Read the audit log** - see every decision the box made.
7. **Ship it in `sbxenv.yaml`** - so the whole team gets the identical box.

The project is already in the workspace - the real `catalog-service-node`: an
Express API over Postgres, with a React frontend and a `.env` full of secrets the
box must keep away from the agent.

```bash
sbx version
```

Everything here is **simulated** with real `sbx` commands - no sandbox, Docker, or
network is touched, so every learner gets the same result.

> [!NOTE]
> The guiding idea: **trust the box, not the agent.** The agent has `sudo` inside
> the sandbox by design - the hypervisor boundary, the proxy, and these policies
> are the controls, not in-VM privilege.

Next: spin the box up - locally and in the cloud.

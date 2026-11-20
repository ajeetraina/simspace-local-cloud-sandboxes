# Pin the Filesystem

Network is sorted. Now the disk: what can the box *see*, and what can it *write*?

By default a box sees one thing - the workspace you pass to `sbx run`. The host
root is read-only; nothing outside the mount exists as far as the agent is
concerned. You can add more, each read-only, by appending `:ro`:

```bash
sbx run claude . ~/shared-libs:ro ~/design-docs:ro
```

- `.` → the $$app$$, mounted **read-write** (the agent's primary workspace)
- `~/shared-libs:ro` → shared code the agent may read but not change
- `~/design-docs:ro` → reference docs, read-only

## See exactly what the box sees

```bash
sbx exec catalog mount
```

```text no-run-button
catalog-service-node  on  /home/agent/catalog-service-node  type virtiofs (rw)
shared-libs           on  /home/agent/shared-libs           type virtiofs (ro)
design-docs           on  /home/agent/design-docs           type virtiofs (ro)
```

Three mounts, and nothing else - the rest of your home directory, your SSH keys,
your other projects simply aren't in the box.

> [!NOTE]
> The workspace is the main **residual risk**: an agent can edit files that run
> later - git hooks, CI configs, build scripts - *within* the writable mount.
> After a session, review `git diff` and check `.git/hooks/` by hand. Keeping
> everything else `:ro` shrinks that surface to just the project.

Next: the filesystem says *where* the agent can write. Cedar says *what actions*
it may take at all.

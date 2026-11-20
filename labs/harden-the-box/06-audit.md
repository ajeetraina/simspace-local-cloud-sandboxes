# Read the Audit Log

Four controls are on. The audit log is how you *see* them working - every decision
the box made, with the policy that made it.

## Network decisions

```bash
sbx policy log
```

```text no-run-button
POLICY LOG - network decisions
  allow   registry.npmjs.org      (allow rule)
  allow   *.pypi.org              (allow rule)
  DENY    evil.example.com        (deny rule - deny beats allow)
  DENY    169.254.169.254         (balanced default - metadata endpoint blocked)
```

Note the last line: the box blocked the cloud **metadata endpoint**
(`169.254.169.254`) on the default policy - a classic credential-theft target - without
you writing a rule for it.

## The full action audit

`sbx policy log` is network only. The audit log is every governed action -
network, file writes, MCP calls, and Cedar decisions - on one timeline:

```bash
sbx audit ls
```

```text no-run-button
AUDIT - catalog  (newest last)
  10:02:14  net     allow   registry.npmjs.org        npm ci
  10:02:17  net     DENY    evil.example.com          blocked by deny rule
  10:02:31  file    allow   write src/routes/products.js
  10:02:44  authz   DENY    read .env                 Cedar forbid (Secrets)
  10:03:02  mcp     allow   catalog-tools__list_skus  static MCP + Cedar permit
```

Read it top to bottom and you can see the whole lab at work: the deny rule, the
Cedar `forbid` on `.env`, the pinned MCP call. Nothing the agent did went
un-adjudicated.

> [!NOTE]
> This is your after-action review. Pair it with the host-side check the sandbox
> can't do for you - `git diff` and `.git/hooks/` - for a complete picture of what
> the session changed.

Next: capture all of this in one file so the team gets the identical box.

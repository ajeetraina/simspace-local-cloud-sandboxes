# Add a Network Policy

The box boots on the **balanced** policy: deny-by-default, with common dev sites
allowed. See what's active:

```bash
sbx policy ls
```

## Allow what the build needs

The $$app$$ installs from npm and PyPI. Allow exactly those hosts - nothing else:

```bash
sbx policy allow network registry.npmjs.org
```

```bash
sbx policy allow network "*.pypi.org,files.pythonhosted.org"
```

A bare host (`registry.npmjs.org`) matches only that host; a wildcard
(`*.pypi.org`) matches subdomains but **not** the root - list both when you need
both.

## Deny what it must never reach

Block a known exfiltration target:

```bash
sbx policy deny network evil.example.com
```

## Prove the rules

Check a host against the policy without the agent having to try it:

```bash
sbx policy check network registry.npmjs.org
```

```bash
sbx policy check network evil.example.com
```

The second comes back **Denied**. And the rule that matters most: **deny always
beats allow**. Even if a host matched an allow rule, an explicit deny wins -
regardless of which is more specific.

> [!NOTE]
> The policy is enforced at the proxy, outside the box. The agent can *try* any
> host; blocked requests never leave the sandbox.

Next: decide what the box can see on disk.

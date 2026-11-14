# The Traditional SDLC

For twenty years the software lifecycle has had a human in every loop. A developer
writes code, opens a pull request, a teammate reviews it, CI runs, someone approves
the deploy. Each arrow is a person making a decision.

```text no-run-button
  Plan  →  Code  →  Review  →  Test  →  Merge  →  Deploy
   👤       👤        👤        🤖       👤        👤
```

Look at the history of the **$$app$$** service and you can see it:

```bash
git log --oneline -5
```

Every commit paused for a human. That's the model security was built around —
**slow by design**, and safe *because* it's slow. Permissions, approvals, code
review, and change control all assume a person is the thing taking the action.

> The trust boundary in the traditional SDLC is **the person**. We trust the
> developer, so we give the developer access.

That assumption is about to break. In the next section the actor taking the
actions is no longer a person — and all the access we handed the person comes
along with it.

Continue to **The Agentic SDLC**.

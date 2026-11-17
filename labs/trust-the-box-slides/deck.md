<!-- chrome: false -->

<img src="assets/slide-01.webp" alt="Trust the box, not the agent — one coding agent, one real job; only the box around it changes" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Open here. The whole talk is one agent, one job, run twice — once loose on the host, once in a box. The agent never gets more trustworthy. The box does.

---

<!-- chrome: false -->

<img src="assets/slide-02.webp" alt="Forty minutes, five parts: Traditional SDLC, Agentic SDLC, the 4Cs, AI Governance, Recap" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Five parts. ~3m on the traditional SDLC, ~7m on the agentic one and its seven problems, ~20m on the four C's and their live demos, ~8m on governance, ~3m to land it. The labs are the hands-on half.

---

<!-- chrome: false -->

<img src="assets/slide-03.webp" alt="Every useful agent has all three: access to private data, exposure to untrusted content, external reach" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: The lethal trifecta. Every agent worth using has all three by design — that's why the fix has to be a boundary at runtime, not a better prompt. You can't train, prompt, or policy-document it out.

---

<!-- chrome: false -->

<img src="assets/slide-04.webp" alt="Meet the Product Catalog — a real Node.js service with Postgres, Kafka, S3 and an inventory API, with secrets plaintext in .env" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: The app is realistic enough to be dangerous: to do its job it needs a DB password, cloud keys, an inventory key, a Docker engine, and network. That's the whole "what weakens isolation" list, forced on stage by one app. github.com/dockersamples/catalog-service-node.

---

<!-- chrome: false -->

<img src="assets/slide-05.webp" alt="The traditional SDLC: a human at every gate — plan, code, build, test, secure, ship" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Twenty years of security assumes a person is the actor at each gate. A human is slow — and that slowness is what caught the bad decision. "Why is there an AWS key in this Dockerfile?"

---

<!-- chrome: false -->

<img src="assets/slide-06.webp" alt="The agentic SDLC: plan, act, observe, repeat — the same six gates, now with no person at any of them" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Same six stages, same app. The only thing removed is the person. Minutes to finish, no human in the loop — everything the person used to catch now ships.

---

<!-- chrome: false -->

<img src="assets/slide-07.webp" alt="The prompt: containerize the Product Catalog, make the tests pass, push an image — YOLO mode, on the host, no human gate" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: This is the prompt, run the way people really run it — YOLO, on the host, "don't ask me." The tests will go green. It will also fail every review the old SDLC had.

---

<!-- chrome: false -->

<img src="assets/slide-08.webp" alt="What the agent did: seven steps, tests green — each reasonable in isolation, each exposing something" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Seven real steps, each reasonable in isolation, tests green throughout. The row numbers carry through to the scoreboard at the end — remember them.

---

<!-- chrome: false -->

<img src="assets/slide-09.webp" alt="Three problems, seven rows: vulnerable images, exposed keys, too much reach" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Group the seven into three. This is the whole problem statement — and each of the four C's will close a specific group. The agent didn't go rogue; it had our keys, our socket, and our network.

---

<!-- chrome: false -->

<img src="assets/slide-10.webp" alt="Speed or safety: pick one — safe but slow vs fast but risky, with real incidents. Refuse the choice." width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: These are real incidents. The usual answer is approve-everything (you're the bottleneck) or YOLO (your access, none of your judgment). We refuse both: fast AND safe, no babysitting.

---

<!-- chrome: false -->

<img src="assets/slide-11.webp" alt="Why not containers? Each capability — credentials, Docker engine, network, host files — weakens isolation" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: A container is a fine boundary for an application. An agent needs credentials, a Docker engine, and the network — and each one weakens the container until it isn't one. Dashed line = a hole in the wall.

---

<!-- chrome: false -->

<img src="assets/intro-to-sandbox.webp" alt="A sandbox is a private machine for your agent — your machine stays out, the agent gets a microVM with its own Docker, and a proxy is the only way out" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Hello, Sandboxes. A sandbox is a private machine for the agent: its own kernel and Docker engine, only the project shared, and a proxy as the single exit. Full freedom inside, a hard wall at the edge. Three ideas carry the rest of the talk — a trusted boundary, you choose what's inside, the whole team reproduces it.

---

<!-- chrome: false -->

<img src="assets/slide-13.webp" alt="Container vs sandbox: whose kernel? Containers share the host kernel; a sandbox is a microVM with its own kernel behind a hypervisor" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: The core difference. An escape from a container targets your kernel; an escape from a sandbox must cross the hypervisor. Sudo inside a sandbox is fine — it's root of a throwaway VM, not your laptop. We took the kernel out of the equation.

---

<!-- chrome: false -->

<img src="assets/slide-14.webp" alt="The 4Cs: one pillar per problem — Contain, Control, Choice, Capacity, each closing specific scoreboard rows" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: The map for the rest of the talk. Four pillars, each closes specific rows from the scoreboard. Each gets one explainer and one live demo. Same agent, same prompt — only the box changes.

---

<!-- chrome: false -->

<img src="assets/slide-15.webp" alt="Contain: the same agent, two worlds — no sandbox vs in a sandbox" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Contain is the boundary itself. Total freedom inside, a hard wall at the edge. The next slide is the live, interactive version — flip it between "no sandbox" and "in a sandbox."

---

<!-- chrome: false -->

<iframe src="labs/trust-the-box-slides/assets/the-box.html" title="The Box — what the agent can reach, with and without a sandbox" style="position:absolute;inset:0;width:100%;height:100%;border:0;background:#fff"></iframe>

Note: DEMO. Start on "No sandbox" — ask it to find passwords, free up disk: it walks right in. Flip to "In a sandbox": only the project came in with it; everything else bonks off the wall. The key never enters the VM, so `COPY . .` has nothing to bake.

---

<!-- chrome: false -->

<img src="assets/slide-16.webp" alt="Contain: three boundaries — the microVM, its own Docker engine, and the host proxy that injects the key, closing rows 2, 3 and 4" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Three boundaries, three rows closed. The microVM hides the host (row 3), its own Docker engine replaces the socket (row 4), and the proxy adds the real key on the way out so it's never in the VM (row 2). The tests still pass; the keys were never in the room.

---

<!-- chrome: false -->

<img src="assets/slide-17.webp" alt="Contain: the key never enters the VM — the agent calls the API with no key, the proxy injects it on the host, row 2 closed" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Zoom in on credential injection. Inside the sandbox `echo $INVENTORY_API_KEY` is empty, yet `curl localhost/inventory/42` returns 200. The agent holds no key, so `COPY . .` runs again with nothing real to bake into the layer. Row 2, closed.

---

<!-- chrome: false -->

<img src="assets/slide-18.webp" alt="Same prompt, new box: sbx run claude — ~/.aws missing, Testcontainers on the sandbox engine, key not in the env; closes rows 2, 3 and 4" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Same prompt, one new word: `sbx run claude`. ~/.aws is missing, Testcontainers use the sandbox's own engine, the key isn't in the env. Rows 2, 3 and 4 — closed by Contain alone.

---

<!-- chrome: false -->

<img src="assets/slide-19.webp" alt="Control: one door, one guard — an allow-list only you can change, every attempt recorded in the policy log" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Control decides where the wall sits. One door, one guard, every attempt recorded. The allow-list lives outside the box — only you can change it. Live demo next.

---

<!-- chrome: false -->

<iframe src="labs/trust-the-box-slides/assets/the-door.html" title="The Door — the guard, the allow-list, and the policy log" style="position:absolute;inset:0;width:100%;height:100%;border:0;background:#fff"></iframe>

Note: DEMO. Send messages to the AI model and GitHub (allowed) and to the news/sketchy hosts (denied). Watch the guard's allow-list and the live `sbx policy log` fill with ALLOW/DENY — the exfil host from row 5 is blocked and recorded.

---

<!-- chrome: false -->

<img src="assets/slide-20.webp" alt="Control: developer policy profiles — the proxy allows npm, Docker Hub and the model API, denies the exfil host, and logs every decision" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Developers pick a profile — Balanced here. The proxy allows the common dev endpoints, denies the exfil host, and `sbx policy log` records every decision. Closes row 5, starts on row 6.

---

<!-- chrome: false -->

<img src="assets/slide-21.webp" alt="Control: organizations add governance — one org policy in Docker Home decides, default-deny on every sandbox, local rules go inactive" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: The organization writes one policy in Docker Home — default-deny, enforced on every sandbox. A local allow rule goes inactive under org policy; blocked requests say "blocked by org policy," so the developer knows who to ask.

---

<!-- chrome: false -->

<img src="assets/slide-22.webp" alt="AI Governance: one policy for network, filesystem and MCP access — default-deny, mountable paths, and Cedar-gated tools" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: One policy, written once in Docker Home, covers three surfaces: network (default-deny allow-list), filesystem (mountable paths, read or write per path), and MCP tools (Cedar — read allowed, write needs approval).

---

<!-- chrome: false -->

<img src="assets/slide-23.webp" alt="AI Governance: teams, sign-in and audit — admin writes one policy, scoped to a team, org-only sign-in, every request in the audit log" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: The governance flow: admin writes one policy, scopes it to a pilot team, requires org sign-in, and the proxy decides every request. The audit log exports to a SIEM — security sees row 5's blocked request, unasked.

---

<!-- chrome: false -->

<img src="assets/slide-24.webp" alt="Demo: the developer goes around org policy — applies the policy, reruns the prompt, the lookalike host is blocked, a local allow rule stays inactive, the audit log shows it" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: DEMO arc. Apply the catalog-team policy, rerun the Act 2 prompt, the agent hits the lookalike host and is blocked by org policy. The developer adds a local allow rule — it stays inactive. The audit log shows the attempt. Developers keep their speed; security keeps the pen.

---

<!-- chrome: false -->

<img src="assets/slide-25.webp" alt="Choice: kits inside the box — swap agent kits and stack mixin kits while the microVM, proxy and policy stay fixed" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Choice is what makes a locked box usable. Same walls, better tools inside: swap the agent kit (Claude Code ⇄ Codex), stack mixin kits (dhi-mixin, scanners). The boundary does not move. Starts closing rows 1 and 6.

---

<!-- chrome: false -->

<iframe src="labs/trust-the-box-slides/assets/the-kit.html" title="The Kit — add kits to the box and harden the base image" style="position:absolute;inset:0;width:100%;height:100%;border:0;background:#fff"></iframe>

Note: DEMO. Dock the agent kit and mixins into the box, then "harden the base" — watch 8 High CVEs drop to 0 while the boundary stays exactly where it was.

---

<!-- chrome: false -->

<img src="assets/slide-26.webp" alt="Choice: tools through one governed gateway — the MCP gateway applies Cedar policy, default-deny, auditing every tool invoke" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Tools reach the agent through one governed gateway: Cedar policy, default-deny, every invoke checked and audited. Reads allowed, writes need approval, unregistered servers denied. Fail closed — no policy loaded means deny all.

---

<!-- chrome: false -->

<img src="assets/slide-27.webp" alt="Choice: pin the agent to DHI with one kit — register the MCP, steer the agent, allow dhi.io, deny third-party registries" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: One declarative kit pins the agent to hardened images: register the DHI MCP, steer the agent (soft), allow dhi.io, and deny third-party registries at the network edge (hard). Steering asks; the deny guarantees — deny beats allow.

---

<!-- chrome: false -->

<img src="assets/slide-28.webp" alt="Demo: same prompt, a zero-CVE base — FROM node:20 (47 CVEs) rewritten to dhi.io/node (0 CVEs), signed, SBOM, non-root; closes row 1" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: The payoff. The agent rewrites `FROM node:20` (47 CVEs, 431 packages, root) to `FROM dhi.io/node` (0 CVEs, 211 packages, SBOM, signed, non-root). Same prompt, better base. Row 1 closed — numbers straight from the lab run.

---

<!-- chrome: false -->

<img src="assets/slide-29.webp" alt="Capacity: one sbxenv.yaml for the catalog repo — every teammate runs sbx env run and gets the identical sandbox; a kit change is a pull request" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Capacity turns one good box into the team's box. One `sbxenv.yaml` in the repo describes agent, kits and resources; every teammate runs `sbx env run` and gets the identical sandbox. A kit change is a pull request. Row 7 closed.

---

<!-- chrome: false -->

<iframe src="labs/trust-the-box-slides/assets/the-file.html" title="The File — one sbxenv.yaml, four identical boxes" style="position:absolute;inset:0;width:100%;height:100%;border:0;background:#fff"></iframe>

Note: DEMO. "By hand": four laptops drift into four different setups. Flip to "From the file": one sbxenv.yaml, four identical boxes. Update once, every sandbox follows — on any machine, laptop or cloud.

---

<!-- chrome: false -->

<img src="assets/slide-30.webp" alt="Capacity: laptop to Cloud Sandboxes — the same file runs locally or fans agents out in parallel in the cloud" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: The same file runs in the cloud — one agent hardens the Dockerfile, another writes tests, another updates the Kafka client, all at once. Two policies: org governance covers local sandboxes; cloud sandboxes use their own network policy (`sbx --cloud policy`).

---

<!-- chrome: false -->

<img src="assets/slide-31.webp" alt="Capacity: measured, not promised — 3/3 features merged, 177s for three features locally, 22,000 tests on a 5-sandbox fleet" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Not a promise, a measurement. Three headless agents merged 3/3 features (32/32 scenarios) in 177s locally; cloud agents ran 31% faster; a 5-sandbox fleet ran 22,000 tests in ~521s vs 1h49m on a laptop. Field evaluation on sbx 0.50.0.

---

<!-- chrome: false -->

<img src="assets/slide-32.webp" alt="Share the box, don't share the writes — the edge of Capacity: two sandboxes writing one shared file or DB lose writes silently; share read-only, write single-writer" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: The edge of Capacity. The same isolation that makes each sandbox safe — its own kernel — is what makes shared writes unsafe: 93–97% of writes lost on a shared file, 494,800 rows acknowledged then gone, locks don't cross VMs. Fix has the same shape as the box: share read-only, write single-writer, with a broker handing out fencing tokens.

---

<!-- chrome: false -->

<img src="assets/slide-33.webp" alt="Scoreboard: every Act 2 row closed — seven rows mapped to Contain, Control, Choice and Capacity" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Seven rows, four pillars, every one closed — and we never made the agent more trustworthy. Point at each row and name its C.

---

<!-- chrome: false -->

<img src="assets/slide-34.webp" alt="Before and after, same prompt — Act 2 agent on the host vs Act 4 governed sandbox, every line flipping from liability to evidence" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Same agent, same prompt, a different box. Read the two columns straight down — base image, credentials, Docker, network, team setup, evidence — every line flips from liability to evidence.

---

<!-- chrome: false -->

<img src="assets/slide-35.webp" alt="Trust the box, not the agent — we never made the agent more trustworthy; we made the box worth trusting. Run the lab at sandboxing.dockerworkshop.com" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Land the title one last time. Point them at the labs that pair with this deck — the same Product Catalog, the same four C's, hands-on at sandboxing.dockerworkshop.com. Thank the room.

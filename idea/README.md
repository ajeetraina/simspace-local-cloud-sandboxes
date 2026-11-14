The story in one line
We hand one coding agent one real job, "containerize the Product Catalog and get it ready to ship", and watch it succeed at the task while failing every security review. Then we keep the same agent and the same job, and change only the box around it, one C at a time.
Hero app: dockersamples/catalog-service-node. The audience already knows it from earlier Docker workshops, and it has exactly the moving parts an agent can break:
• A Node.js REST API that stores products in PostgreSQL
• Product images in an S3 bucket (LocalStack in dev)
• Inventory data from an external service (WireMock in dev)
• Product updates published to Kafka
• Integration tests that use Testcontainers to start Postgres, Kafka and LocalStack
• Dev tools: pgAdmin on 5050, kafbat on 8080, demo client on 5173
Why this app works for the talk: to do its job, the agent needs a database password, cloud credentials, an inventory API key, a Docker engine for Testcontainers, and network access to npm and Docker Hub. That is the full list of things the "Why not containers?" slide says weakens isolation. The app forces the problem on stage without any staging tricks.
The tagline that runs through every slide: the agent never gets smarter or more trustworthy. Only the box changes.
Act 1: The traditional SDLC (about 3 minutes)
In the old loop, a human stood at every gate, so a bad decision got caught by a person before it shipped.
Walk the Product Catalog through the loop the audience knows:
1.  Plan: a ticket says "ship the catalog service as a container".
2.  Code: a developer writes the Dockerfile and compose.yaml by hand.
3.  Build: Docker Build (or Build Cloud in GitHub Actions, as the sample repo does).
4.  Test: Testcontainers starts Postgres, Kafka and LocalStack for integration tests.
5.  Secure: a scan runs, a reviewer reads the PR, a platform team owns the base image.
6.  Ship: push to the registry, deploy.
The line to land: speed was limited by humans, and so was the blast radius. One developer, one laptop, a few commits a day, and a reviewer who asks "why is there an AWS key in this Dockerfile?"
Visual idea: the classic SDLC loop with a small person icon at each gate.
Act 2: The agentic SDLC (about 7 minutes)
The agent finishes the ticket in minutes, the tests go green, and the result would fail every review the old SDLC had. That is the turn of the story.
The prompt on screen: "Containerize the Product Catalog service, make the integration tests pass, and push an image. Don't ask me questions, I'm in a meeting." Run it in YOLO mode on the host, the way many developers really do.
What the agent does, step by step (seed these in the simspace so every run tells the same story):
#
What the agent did
Why it seemed reasonable
What it exposed
Fixed by
1
Wrote FROM node:latest (full Debian image)
"Latest" works and has every tool
Hundreds of packages and a long CVE list shipped to prod
Choice (hardened base via DHI kit)
2
COPY . . with no .dockerignore
Simplest way to get the code in
.env with AWS keys, the inventory API key and a payment test key baked into an image layer
Contain (credential injection)
3
Read ~/.aws/credentials to "fix" the S3 test
LocalStack failed, real S3 looked like a fallback
Real cloud keys in the agent's context and logs
Contain (microVM, only the project folder)
4
Mounted /var/run/docker.sock for Testcontainers
Testcontainers needs a Docker engine
Full control of the host Docker, every other container
Contain (its own Docker engine)
5
npm install of a lookalike package a README suggested
Prompt injection in a dependency's docs
Install script that posts env vars to an unknown host
Control (network policy, blocked and logged)
6
Hard-coded POSTGRES_PASSWORD=postgres in compose.yaml and pushed to a public repo
It is the dev default
A known password in a public image
Control + Choice (org policy, scanner kits)
7
Each teammate's agent did it a little differently
No shared setup
Six laptops, six different risk profiles
Capacity (sbxenv.yaml)
Group the seven into three problems for the slide (this is the "agents are busy" picture):
• Vulnerable images: rows 1 and 6. The agent picks what works, not what is safe.
• Exposed product keys and credentials: rows 2 and 3. The agent can read whatever you can read.
• Too much reach: rows 4, 5 and 7. The agent can reach whatever your laptop can reach, and every laptop is different.
The line to land: "The agent did not go rogue. It did exactly what we asked. The problem is that it had our keys, our socket and our network while it did it."
Tie back to the deck: this is the "Each new capability weakens the container's isolation" slide, now with a real app attached to every row.
Act 3: Same agent, same job, a better box (the 4Cs, about 20 minutes)
Rerun the exact prompt from Act 2 inside a sandbox, and fix one group of problems per pillar. Each pillar ends with the scoreboard: which rows from the Act 2 table are now closed.
01 Contain: the agent can't take what it can't see
Closes rows 2, 3 and 4. Run sbx run claude from the catalog-service-node folder. Same agent, same prompt.
• MicroVM: the agent gets the catalog folder and nothing else. cat ~/.aws/credentials returns nothing, because that file lives on the host, not in the VM.
• Its own Docker engine: Testcontainers still starts Postgres, Kafka and LocalStack, but on the sandbox's engine. Retry the socket escape from the deck: it only reaches the sandbox's own Docker.
• Credential injection: the inventory API key and model key are stored on the host and added by the proxy on the way out. The agent can call the inventory service; it can never print the key. Even if COPY . . runs again, there is no real .env secret to bake into the layer.
Line to land: "The tests still pass. The keys were never in the room."
02 Control: decide where the walls sit
Closes row 5, and starts on row 6. Developer layer first, then the organization layer.
• Developer: Balanced profile by default. The lookalike package's install script tries to post env vars to an unknown host and is denied. Show sbx policy log: the denied request is there with its destination.
• Allow what the job needs: registry.npmjs.org, Docker Hub, the model API. One sbx policy allow network rule if the agent hits something legitimate.
• Organization: hand off to the AI Governance section below. The admin's policy now decides, not each developer.
Line to land: "Every exit goes past the front desk, and the desk writes it down."
03 Choice: put the right tools inside the box
Closes rows 1 and 6. The box is safe; now make it smart about images.
• Agent kit: start with Claude Code, then swap to Codex on the same project to show the boundary does not move.
• Mixin kits for supply chain: a Docker Hardened Images (DHI) MCP kit so the agent can look up CVEs, packages and attestations before choosing a base. The agent rewrites FROM node:latest to a DHI Node image and the CVE count drops sharply.
• Scanner kits from partners: your Snyk, Black Duck, Sonar or Mend kits run inside the same box and flag the hard-coded Postgres password before the push.
Line to land: "Same walls, better tools inside them."
04 Capacity: one box for the whole team, laptop to cloud
Closes row 7. Commit an sbxenv.yaml to the catalog repo: agent, workspace, DHI and scanner kits, CPU and memory.
• Every teammate runs sbx env run and gets the identical sandbox. A kit change is a pull request.
• Laptop to cloud: the same environment file runs on Cloud Sandboxes. Fan out several agents at once, for example one hardens the Dockerfile, one writes missing tests, one updates the Kafka client.
• Be honest on stage: org governance today applies to local sandboxes; cloud sandboxes use their own network policy (sbx --cloud policy). Say this out loud rather than letting the audience assume one policy covers both.
Line to land: "Update once, every sandbox follows, on any machine."
Scoreboard slide
Act 2 problem
Rows
Pillar that closes it
Vulnerable images
1, 6
Choice (DHI and scanner kits), Control (org policy)
Exposed product keys and credentials
2, 3
Contain (microVM, credential injection)
Too much reach
4, 5, 7
Contain (own engine), Control (network policy), Capacity (sbxenv.yaml)
AI Governance: the security team's view of the Product Catalog (about 8 minutes, inside Control)
With AI Governance, the platform team writes one policy for the catalog team in Docker Home, and every developer's sandbox follows it, even if a developer tries to loosen it locally.
The admin writes the rule once; every request from the agent passes the proxy, and each decision lands in the audit log the security team reads.
What it controls
Control
What it does
Product Catalog example
Network access
Which destinations sandboxes can reach. Default deny.
Allow registry.npmjs.org, Docker Hub, dhi.io, the model API, github.com. Everything else is blocked, including the exfil host from row 5.
Filesystem access
Which host paths developers can mount as workspaces (read or write)
Only ~/src/catalog-* can be mounted. Nobody mounts ~ or a folder holding .aws.
MCP access
Which MCP servers can be registered, which tools, resources and prompts can be used, and approval gates (Cedar policy)
Allow the DHI MCP server's read-only tools. A Postgres MCP server can read the catalog DB, but any write needs approval.
Licenses and teams
Policies apply to licensed members, org-wide or per team
A "catalog-team" policy for the pilot, the org-wide baseline for everyone else
Sign-in enforcement
Only org members can use sandboxes on managed machines
Closes the "I signed in with my personal account" gap
Audit logs
Records the policy decision for each sandbox request; view and export in Docker Home or forward to a SIEM
The security team sees row 5's blocked request without asking the developer
How it behaves (the facts to say on stage)
• Managed in Docker Home, or programmatically through the AI Governance API on hub.docker.com/v2 (/orgs/{org_name}/governance/policies). Show one API call so platform teams see it fits GitOps.
• Organization policy is default deny: sandboxes reach, mount and use only what the policy allows.
• When org governance is active, local and kit allow rules stop applying. They still show in sbx policy ls, marked inactive.
• Blocked requests say "Blocked by org policy", so the developer knows who to ask.
• Policy changes can take up to five minutes to reach developer machines. Mention it before the demo so a delay does not look like a bug.
• It is a separate paid subscription. Say so plainly; the audience will ask.
Demo beat: the developer tries to go around it
1.  As admin, apply the catalog-team policy in Docker Home.
2.  As developer, run the Act 2 prompt again in a sandbox.
3.  The agent tries the lookalike package host. Blocked by org policy.
4.  The developer adds sbx policy allow network for that host. sbx policy ls shows the local rule as inactive. Still blocked.
5.  Switch to the admin view: the audit log shows the blocked request and the policy decision.
Line to land: "Developers keep their speed. The security team keeps the pen."
Act 4: Who approved that build? (about 3 minutes)
End by replaying Act 2 side by side with the governed run, so the audience sees the same prompt produce a safe result.

Act 2: agent on the host
Act 4: agent in a governed sandbox
Base image
node:latest, long CVE list
DHI Node image, CVE count near zero
Credentials
AWS and inventory keys in the image layer
Keys injected by the proxy, never in the VM
Docker
Host socket, full control
Sandbox's own engine
Network
Anything, including the exfil host
Org allowlist, blocked requests logged
Team setup
Different on every laptop
One sbxenv.yaml, laptop or cloud
Evidence
None
Audit log of every policy decision
Closing line: "We never made the agent more trustworthy. We made the box worth trusting. Trust the box, not the agent."
Call to action: run the simspace lab, read the governance docs, try a kit from Docker Hub.
Slide outline and simspace lab modules
About 26 slides for a 40 to 45 minute slot; "Reuse" means the slide already exists in Intro to Docker Sandboxes and only needs Product Catalog labels.
#
Slide
Source
1
Title: Trust the Box, Not the Agent
New
2
Agenda: SDLC, Agentic SDLC, 4Cs, Governance, Recap
Reuse slide 2, retitled
3
Meet the Product Catalog (architecture)
New
4
The traditional SDLC, a human at every gate
New
5
The agentic SDLC, plan act observe repeat
Reuse slide 4
6
The prompt: "containerize it, don't ask me"
New
7
What the agent did (the 7-row table)
New
8
Three problems: vulnerable images, exposed keys, too much reach
New
9
Why not containers? Each capability weakens isolation
Reuse slide 8, rows relabelled for the catalog
10
Hello, Sandboxes: a private machine for your agent
Reuse slide 10
11
The 4Cs
Reuse slide 11
12
Contain: three boundaries
Reuse slide 13
13
Contain: the inventory key never enters the VM
Reuse slide 14
14
Demo: Contain
Reuse slide 15
15
Control: developer policy profiles
Reuse slide 17
16
Control: organizations add governance
Reuse slide 18
17
AI Governance: network, filesystem, MCP
New
18
AI Governance: licenses, teams, sign-in enforcement, audit
New
19
Demo: developer tries to go around org policy
New
20
Choice: kits, DHI and scanner mixins
Reuse slide 22
21
Demo: node:latest to DHI, CVEs down
New
22
Capacity: sbxenv.yaml for the catalog repo
Reuse slide 28
23
Capacity: laptop to Cloud Sandboxes
Reuse slide 30
24
Scoreboard: every Act 2 row closed
New
25
Before and after side by side
New
26
Trust the box, not the agent + get started
Reuse slide 33
Simspace lab modules
One module per act, so the lab and the talk tell the same story. No model API keys or third-party sign-ups needed; it all runs as a simulated terminal.
1.  Meet the catalog: clone, docker compose up, see Postgres, Kafka, LocalStack, WireMock.
2.  YOLO on the host: replay the agent's 7 steps; learner finds the leaked .env in an image layer and the CVE list.
3.  Contain: same prompt in sbx run; learner checks ~/.aws is missing, Testcontainers runs on the sandbox engine, and the key is not in the env.
4.  Control (local): blocked exfil, read sbx policy log, add one allow rule.
5.  Control (org): apply a catalog-team policy, watch the local rule go inactive, read the audit entry.
6.  Choice: add the DHI MCP kit, swap the base image, compare CVE counts.
7.  Capacity: commit sbxenv.yaml, start it locally, then run it on Cloud Sandboxes.
8.  Recap quiz: match each leaked item to the C that closed it.
Check before stage
[ ] The .env keys, lookalike package and exfil host in Act 2 are seeded lab props, not things in the real sample repo. Say so once on stage.
[ ] Confirm the exact credential command for the inventory key in your current sbx version (sbx secret import changed in v0.35).
[ ] Confirm the DHI MCP kit name and the real CVE numbers for node:latest vs the DHI Node image on demo day; put the real numbers on slide 21.
[ ] Confirm whether org governance covers Cloud Sandboxes by talk day; today the docs say it applies to local sandboxes only.
[ ] Confirm audit log fields you will show on screen, from a real Docker Home export.
[ ] Check the Docker Home menu path for policies (one docs page says AI Platform in the left navigation).
Sources
• dockersamples/catalog-service-node
• Product Catalog tech stack (Collabnix workshop)
• Organization governance
• Get started with organization governance
• Sandbox governance overview
• Organization policies: network, filesystem, MCP
• Docker AI Governance API
• Intro to Docker Sandboxes deck (your upload), slides 2 to 33

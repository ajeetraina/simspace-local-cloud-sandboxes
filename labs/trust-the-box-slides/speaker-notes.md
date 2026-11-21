# Trust the Box — Speaker Notes

_All speaker notes in reading order, one section per slide. Generated from `deck.md`._


## 1. Trust the Box, Not the Agent

Hey everyone, thanks for coming. Today is all about Docker Sandboxes. By the end of this session I want you walking out with one clear picture in your head: a box you can trust, that you get to fill with whatever you need, and that your whole team can share. That's it. That's the talk. Everything else is details and demos, and there are plenty of demos, so if something breaks live, you'll get to watch me sweat in real time.


## 2. Whale, hello there

Very quickly, so you know who's talking at you. My name is Ajeet Singh Raina, I'm a Developer Advocate at Docker. I've spent a little over twenty years in this industry — systems integration, testing, consulting, and developer relations. I was a Docker Captain before I joined the company, and I run a developer meetup in Bengaluru that's grown to about seventeen thousand people. I also co-wrote a book called Operational AI with Docker , which is honestly this talk stretched out to a few hundred pages. But you didn't come here for my résumé. You came here because somewhere in the last few weeks, an agent did something on your machine — or on a machine you're responsible for — that made your stomach drop just a little. Let's talk about that feeling, and then let's


## 3. Agenda

Here's the plan. We'll start with the problem, because sandboxes only make sense once you feel the pain. Then a quick detour: why not just use containers? Spoiler, I'm going to break out of one live. Then we'll say hello to sandboxes and walk through four pillars: Contain, Control, Choice, and Capacity. If you caught the WeAreDevelopers keynote, these will sound familiar. They're a really nice way to think about the whole thing.


## 4. The problem: agents that act

So let's start with the problem. Everybody wants the productivity agents promise. Nobody wants to be the person whose agent did something weird on a Friday afternoon. How do we get the first thing without the second?


## 5. Agents need access to do real work

Agents earn their keep by acting: reading code, running builds, installing packages, calling APIs. Freedom is the feature.


## 6. More access means a bigger blast radius

Same agent, same mistake, very different outcome depending on what it can reach. Developers want agents; security teams want control; today they cannot have both.


## 7. Coding agents act on their own

Here's what makes an agent an agent. It plans, it acts, it looks at what happened, and it does it again. Over and over. Hundreds of times. And the whole point is that you're not in that loop. That's the magic. It's also the problem. Because when that loop goes sideways, it can delete files outside your project, it can go poking through your keys and tokens, it can send data somewhere you've never heard of, or it can happily install a package that turns out to be malicious. Not because it's evil. Just because it's trying really hard to finish the task.


## 8. A Product Catalog sample demo

So this is the app we'll be securing all the way down that dev-to-prod road. It's a Product Catalog service, and it's deliberately realistic rather than a toy. In the middle you've got the catalog-service application, and it's not living alone: it writes product data to PostgreSQL, pushes product images to AWS S3, and publishes product updates through Kafka. Reaching outside its own boundary, it talks to an Inventory service and other downstream services. The point I want you to take away is that this is a real supply chain of moving parts - every one of those boxes is something we eventually have to trust and prove. Before we talk about how to fix any of it, I want you to feel it yourself - so let's try.


## 9. The traditional workflow

Here's the world we grew up in - the traditional workflow. A human writes, reviews, and ships at every stage, and the key line is at the top: the attack surface is only what you choose to pull. You've got the inner loop on the left - code, open source, build, test - the fast cycle on your own machine, and every icon around it is a person. Then you push, and it flows into the outer loop on the right - integrate, test, deploy - again, humans at every point. It's a chain of deliberate human decisions from left to right. Hold this picture in your head, because the next slide keeps the exact same map and swaps out just one thing.


## 10. The Agentic Workflow

Same map, same inner and outer loops, same push in the middle - but look what changed: every human icon is now an AI agent. That's the whole point. In the agentic workflow, an agent sits at every single stage - code, open source, build, test, integrate, deploy - and so the attack surface is no longer just what you pull. It's every autonomous action, every tool call, every credential those agents touch across the entire road from inner loop to production. That's a lot of green robots and a lot of blast radius - the same failure mode as that 2:47 AM commit, now at every stage. Let me make it concrete - here's an agent turned loose on a build with no guardrails at all.


## 11. Agents expand your attack surface automatically

Here's why this matters. Traditional attacks had to target a developer making a conscious choice — someone had to decide to pull that package, run that script. Agentic systems automate the choosing. The agent pulls and installs without a human reviewing each step, so there's no pause where a person goes "wait, that looks off." And here's the uncomfortable part: the better the agent gets, the more it does on its own — which means the bigger the blast radius. Capability and risk scale together.


## 12. Agents expand your attack surface automatically

Same idea, side by side. On the left, the traditional workflow: manual, reviewed, a human gate at every step. On the right, the agentic one: autonomous, no review, the agent acting end to end. It's the same work getting done — but every one of those review gates on the left is just gone on the right. That's not a small tweak to the threat model; it's a different threat model. So the rest of this talk is about putting one gate back — in a place the agent can't remove.


## 13. What agents are already doing in your pipeline

And this isn't the future. It's already in your pipeline. Agents pull dependencies from registries you never audited. They generate Dockerfiles, often with the first base image they see. They edit infrastructure: Terraform, Helm values, Compose files, IAM policies. And they trigger builds on their own, opening PRs, pushing branches and kicking off pipelines. So, what can go wrong?


## 14. The ungoverned agent

Here's what that looks like with no guardrails. The agent on your host, full permissions, asked to containerize the app. It reaches for the first base image it's ever seen — FROM node:20 — and ships it. Look at the scorecard: six High CVEs, 431 packages it pulled in without asking, no SBOM so nobody can tell what's actually in there, and it's running as root. Nothing here is malicious. The agent did exactly what you asked. It just had no reason to pick anything better, and nothing stopping it. Hold that scorecard in your head — six Highs, 431 packages, root — because we're going to run this exact prompt again later inside a sandbox and compare.


## 15. Set up

Here's the setup for the demo, so you can follow along. Clone the Product Catalog showcase repo, and notice what's not there: no Dockerfile, no Compose file. Then ask the agent to containerize it for production, and scan whatever it builds with docker scout quickview. On the ungoverned run that scorecard comes back ugly — a stack of Critical and High CVEs. That's our baseline, and it's the number we're going to beat later inside a sandbox.


## 16. Demo time!

Demo time. Cue: on the host, no sandbox. Prompt the agent to containerize the Product Catalog. Let it write the Dockerfile and build. Then scan the image and read the scorecard out loud — FROM node:20, the High CVEs, the package count, running as root. Don't fix anything yet. The point is to show the default, ungoverned result so we can beat it later.


## 17. Developers have to choose between speed and safety

So today, developers are stuck picking a side. On one end, you approve every single action. Very safe. Also very slow, and you've basically become a full-time button clicker. On the other end, YOLO mode. Skip all the permissions, let it rip. Super fast, but now the agent has exactly the same access you do. Everything you can touch, it can touch. And that's where you hear people say things like, I'm babysitting my agents. Nobody signed up to be a babysitter. We want the right side's speed with the left side's safety.


## 18. Why not containers?

Okay. Some of you are already thinking: we're at a Docker talk, just put the agent in a container. Fair! Containers are great at isolating applications. But agents aren't applications. They need a lot more access to be useful. Let me show you what happens.


## 19. Container vs sandbox: whose kernel?

Here's the difference that matters most, and it comes down to one question: whose kernel? An ordinary container shares the host's kernel. There's one kernel, and everyone — your Mac, every container — is talking to it. A Docker Sandbox is a microVM: it boots its own Linux kernel, completely separate from yours. Why do you care? Because it changes what "root" means. Root inside a container is a short step from root on your host — same kernel, so a kernel-level escape lands on your machine. Root inside the sandbox is root on a throwaway Linux VM that has no path to your Mac. So the agent can sudo all it likes in there, and "admin" inside the box is simply not admin on your laptop.


## 20. Why not put the agent in a container and restrict it in a boundary?

So the obvious move is: put the agent in a container, lock it down, draw a nice boundary around it. And that almost works — right up until the agent needs to build or run containers itself, which coding agents do constantly. To let it do that, you mount the Docker socket in. And that one mount quietly hands the keys back. Anything that can talk to the Docker socket can start a new container with the whole host mounted inside it — so the "restricted" container can reach straight past its own boundary to your filesystem. The boundary's still drawn on the slide; it just doesn't hold. Let me show you how fast that goes.


## 21. Demo time!

Demo time. Cue: start a container with the Docker socket mounted — the setup everyone reaches for. From inside it, launch a second container with the host root filesystem bind-mounted. Now read a file off the host — something that was supposed to be outside the boundary. That's the escape: no exploit, no CVE, just the socket doing exactly what it's designed to do. Land the line: the container didn't fail, it was never a boundary in the first place.


## 22. Each new capability weakens the container's isolation

So what just happened? Here's the pattern. Every time you give the agent something useful, you chip away at the box. Mount your source code? Now it can change files on your host. Give it network access? It can reach anything, internal or external. Pass in API keys as environment variables? It can read them and send them wherever it likes. And mount the Docker socket? Well, you just saw that one. Isolation is basically gone. The more useful you make the agent, the less isolated the container actually is. That's not a bug in containers. It's just not what they were built for.


## 23. Hello, Sandboxes

So we need something different. Something where you can give the agent everything it needs, but it still can't get out. Say hello to Docker Sandboxes.


## 24. A sandbox is a private machine for your agent

Here's the mental model, and if you remember one slide, make it this one. Think of a sandbox as a private machine just for your agent. On the outside is your laptop with all your stuff: your SSH keys, your other projects, your own Docker. Inside the sandbox there's a microVM, and that's where the agent lives. It gets your project folder, and only that folder. It even gets its own Docker engine, so it can build and run containers all day without ever touching yours. And when it wants to talk to the internet, there's exactly one way out: through the proxy. Three ideas fall out of this. A trusted boundary. You choose what goes inside. And the whole team can reproduce the same setup.


## 25. The Box

This one's interactive — poke at it while I talk. On the left, "No sandbox": the agent is sitting on your computer with everything you own in reach — your project, your photos, your passwords, your documents. Ask it to do something and watch what it can touch. Now flip to "In a sandbox" and run the same request: it only ever sees the project folder. Same agent, same ask, completely different blast radius. That's Contain, as a toy you can click.


## 26. Four pillars structure the rest of the session

We'll organize the rest of the talk around four pillars. Contain is the boundary itself: the microVM and the guarded network. Control is where you decide where that boundary sits, and where organizations can layer on governance. Choice is about choosing what goes inside, using kits. And Capacity is about making that same sandbox show up for every developer and every run. The first two give you the trusted boundary, the third is your choice, and the fourth is sharing.


## 27. A sandbox gives you four things

Same four pillars, said in plain English so they stick. Contain is the wall — the part the agent can't push through. Control is what it's allowed to reach through that wall. Choice is what you put inside with it. And Capacity is how you give that exact same box to everyone else. Wall, reach, inside, sharing. If the four C's don't land, those four words will. Let's take them one at a time.


## 28. Contain

Pillar one, Contain. Let's look at the actual boundaries that keep the agent's actions inside the box.


## 29. Three boundaries surround every agent

There are three boundaries, and I like to think of them like a building. Number one, the microVM. That's a separate room with its own walls. Its own kernel, its own filesystem, its own Docker. Your host just isn't reachable from in there. Number two, the network proxy. That's the front desk. Every request heading out passes the desk, and the desk checks it against policy. Number three, credential injection. The front desk also holds the keys. The agent never carries them. When a request needs a key, the desk adds it on the way out. So inside the room, the agent can go wild. It's the walls and the desk that keep everybody else safe.


## 30. Credentials are added outside the sandbox

Let's zoom into that credential bit, because it's honestly my favorite part. The agent wants to call some API. It sends the request with no key at all. The proxy catches it and asks, is this destination allowed? If not, it stops right there. If it is, the proxy grabs the key that's stored on your host and attaches it. The API answers, and the agent gets the result. But the key itself? Never went into the sandbox. So even if the agent gets tricked into dumping every secret it can find, there's nothing to dump.


## 31. Demo time!

Demo time — let's poke the three boundaries from inside the sandbox. Cue one, the microVM: try to reach a file on the host, or check the kernel, and show there's nothing of yours in there. Cue two, the proxy: curl something the policy doesn't allow and watch it get refused. Cue three, credential injection: show the agent has no key in its environment, then make a call to an allowed API and watch it succeed anyway — the proxy added the key on the way out. Walls, front desk, keys — all three, live.


## 32. Control

Pillar two, Control. Contain gives us walls. Control is about deciding where those walls sit, so the agent can actually get work done.


## 33. Developers decide what the agent can reach

As a developer, you pick how tight the network is. There are three profiles. Locked Down is the strictest: only what you explicitly allow. Balanced is the default: the common dev stuff like package registries just works, and everything else is denied. Open lets outbound traffic through but still logs it. On top of any profile, you can add your own allow and deny rules. And because everything goes through the proxy, everything gets logged. Run sbx policy log and you can see exactly what the agent tried to reach. It's weirdly satisfying, and occasionally a little alarming.


## 34. The Door

And here's the proxy, interactive. The agent's inside the sandbox, and the only way to the internet is through one door with a guard. The guard checks an allow-list — the AI model and GitHub are on it, the sketchy site isn't — and every attempt, allowed or denied, shows up in sbx policy log. The key detail: you author that list from outside the box. The agent is inside; it can't edit its own guard. Try sending it somewhere that's not on the list and watch the door hold.


## 35. Organizations add governance on top

Now, if you're an organization, you probably want more than every developer doing their own thing. That's where AI Governance comes in. Think of it as layers. At the bottom, Contain is always on. MicroVM, proxy, credential injection, no matter what. In the middle, developers set their local policy. And on top, the organization can set central policy, collect audit logs, and say which agents and kits are approved, enforced across every sandbox. So the security team gets visibility and guardrails, and developers still get to move fast.


## 36. Live demo

Demo time. Cue: have the agent hit an endpoint that's blocked. Show it fail. Pull up the policy log and find the denied request. Allow it, and let the agent carry on.


## 37. Choice

Pillar three, Choice. So we've got a nice locked-down box. Great. But a locked-down box with none of your tools in it isn't very useful. How do you make it work for your team, with your agent, your tools, your endpoints?


## 38. Security tools usually trade away flexibility

Here's the thing about most security tools. They live on this diagonal. The more secure something is, the less flexible it gets. A locked VM with no network is super secure and super annoying. Run the agent right on your host and you get total flexibility and basically zero security. Containers sit somewhere in the middle and slide down the line as you add capabilities. Sandboxes break out of that line. The security comes from the microVM and the proxy, and those never change. What runs inside is completely up to you. So you don't have to pick.


## 39. Kits define what runs inside the boundary

The way you fill the box is with kits. There are two flavors. Agent kits are the harness: which coding agent runs and how it starts up. You get one per sandbox. Mixin kits are everything else: tools, credentials, network rules. Stack as many as you want on top of any agent. And kits can come from basically anywhere. Docker Hub, any container registry, a Git repo, or just a folder in your project. Underneath all of it, the microVM and proxy stay exactly the same.


## 40. Swap the agent, add the tools

And this is where it gets fun, because it's really just mix and match. Pick one agent. Claude Code, Codex, Hermes, Pi, whatever your team likes. Then add whatever tools you need: GitHub over SSH, SSH commit signing, your project's dev environment setup, maybe code-server if you want VS Code in the browser. Out comes your sandbox. Don't like the agent? Swap it. The tools stay. Need another tool? Add it. The agent stays. And either way, the boundary doesn't budge.


## 41. Agents talking to an MCP server

Quick word on how the agent actually uses outside tools, because this sets up the next few slides. The things we keep calling "tools" — a database, GitHub, Stripe, a browser, a hardened-image lookup — the agent reaches through MCP servers. And the key design choice is that the agent doesn't connect to each of those servers itself. It talks to one governed gateway, and the gateway talks to the servers. One door in, one place to apply policy. Keep that single-door picture in mind; everything in the Choice demos runs through it.


## 42. Agent with a sandbox

Same prompt as our ungoverned agent — containerize the app — but this time it runs inside a sandbox, with its own daemon and its own network. The one thing that changed: before it writes a single FROM line, it asks the DHI MCP server for a hardened base, and it gets back dhi.io/node. Read the scorecard out loud: 211 packages, and zero across the board — zero critical, zero high, zero medium, zero low. SBOM attached, signed, running non-root. Same agent, same prompt, just a better tool within reach.


## 43. A governed agent

Now zoom out from that one result to the whole governed agent. Three things surround it. On one side, the MCP gateway and its catalog — the single door to every external tool, including the DHI lookup we just used. Underneath, the network proxy carrying policy and secrets, so credentials get added on the way out and never sit inside the box. And around the workspace, filesystem rules that keep the agent to the project folder. That zero-CVE Product Catalog image isn't luck — it's what falls out when all three are in place at once.


## 44. A governed agent

Same picture, stripped to the three pieces that matter: the MCP gateway for tools, the network proxy for policy and secrets, and filesystem policy for the workspace — all wrapped around the Product Catalog with its zero-CVE base. This is the shape of a governed agent. Everything we've built in the Choice section lives in one of those three boxes. Hold that shape, because next we look at how the gateway actually enforces what the agent is allowed to do.


## 45. The agent talks to one gateway, never to servers directly

The one idea everything hangs on: the agent never talks to those tool-servers directly. It talks to one gateway, and the gateway talks to the servers. That single chokepoint is where authorization, credentials, and audit happen. Every command in the runbook is really just "set up that gateway path" or "manage what's behind it."


## 46. Point it at a real gateway, nothing else works

There are two ways to stand up that gateway. A local gateway you run yourself, right next to the sandbox — great for trying this out on your own machine. Or a hosted control plane, where the policy lives centrally and every sandbox in the org points at it. Either way, the important property is the same: it's fail-closed. If the agent isn't pointed at a real gateway, nothing works — tool calls don't fall back to some open path, they just fail. That's deliberate. You can't accidentally end up ungoverned. And the policy itself isn't buried in config on someone's laptop; it's authored in Docker Hub, so there's one place that decides what's allowed.


## 47. Default-deny allow-list over (server, tool), authored in Cedar

And here's what that policy actually says. It's a default-deny allow-list, and the thing being allowed isn't a whole server — it's a specific tool on a specific server. So you're not saying "GitHub is allowed," you're saying "the create-pull-request tool on the GitHub server is allowed, nothing else." It's written in Cedar, the same policy language AWS uses for authorization, so it's a real grammar, not a pile of if-statements. You author it once and it syncs everywhere. And anything the policy doesn't explicitly permit is both denied and audited — so you get the block and the record of the attempt. Allow-list, per tool, written once, enforced everywhere.


## 48. One feature: agents in a sandbox using external tools safely

This is really a runbook — the operations guide for one feature: letting an AI agent running inside a Docker sandbox use MCP servers (external tools — Notion, Stripe, GitHub, a browser, etc.) safely. It answers four practical questions: what tool-servers are supported, how you register them, how the sandbox connects to them, and how you manage their credentials. That's the whole scope.


## 49. Same agent, same prompt

This is the payoff slide — the before and after, side by side, same agent, same prompt. On the left, the ungoverned run from earlier: FROM node:20, two Critical, twelve High, twenty Medium, thirteen Low. On the right, the same agent with the DHI MCP server available. Before it writes a single FROM line, it asks the server for a hardened base and gets back dhi.io/node:24-debian13 — zero CVEs, signed, with an SBOM. Nothing about the agent changed. We didn't write a better prompt or add a linter. We just gave it a better tool to reach for, behind the governed gateway, and the result went from forty-seven vulnerabilities to zero. That's the whole argument for Choice in one picture.


## 50. Demo time!

Demo time — filling the box with kits. Cue: start a sandbox with an agent kit plus a mixin or two pulled straight from Docker Hub — the DHI MCP kit is the one to show here. Point out that the agent came from one kit and the tooling from another, stacked on top, no custom image built. Then re-run the containerize prompt and show the agent reaching for the hardened base through the gateway — the zero-CVE result from the previous slide, live. Land it: same boundary, we just chose what went inside.


## 51. You can build your own kits

And yes, you can build your own. Kits follow an open spec, and the repo has some examples to crib from. If your team has a weird internal tool or a custom agent, you can package it up exactly the same way. But building kits is really its own session, so I'll just leave the link here and move on before I get too excited.


## 52. Capacity

Last pillar, Capacity. One developer with one nice sandbox is a great start. But the real payoff is when your whole team, and your automation, gets the exact same sandbox every single time.


## 53. Scaling up starts with reproducibility

And the secret to scale is reproducibility. One developer setting things up by hand? Totally fine. Now do that across a team. Somebody's on an old kit, somebody forgot a tool, somebody else has a slightly different setup, and suddenly nothing quite matches. Before you can run lots of sandboxes, you need every sandbox to be the same. And that means describing it once, in a file.


## 54. An environment file describes the whole sandbox

That file is the sandbox environment file, sbxenv.yaml. It's pretty small. You say which agent to run, which folder is the workspace, which kits to pull in, and how much CPU and memory to give it. Notice what's not in here: no network rules. That stuff comes with the kits. Then you run sbx env run, it shows you a plan, you approve it, and you're in. Heads up, this is experimental right now, so the format may still change.


## 55. Commit it once and the whole team shares one setup

And here's the payoff. That file goes in your repo. Need to update a kit, add a new one, or swap the harness? Change the file. Open a pull request. It gets reviewed like any other code. Merge it. And the next time anyone on the team starts their sandbox, they get the new version. Update once, and every sandbox follows. No more wiki page titled how to set up your agent, last updated two years ago.


## 56. Cloud Sandboxes run the same setup remotely

Quick peek at what's coming. Right now your sandbox runs on your laptop, so you're limited by your laptop's CPU and memory. Cloud Sandboxes take that same environment file and run it on Docker's infrastructure instead, so you can spin up more sandboxes than one machine could ever handle. It's early access and not ready for organization use yet, so I won't spend long here. But notice it's the same file. That's why reproducibility matters so much.


## 57. Demo time!

Demo time — the team story. Cue: open the sbxenv.yaml in the repo and walk the four lines — agent, workspace, kits, resources. Run sbx env run, let it print the plan, approve it, and you're inside the exact box we've been building all talk. Then make the point land: this file is committed, so the person next to you runs the same command and gets the same sandbox — same agent, same kits, same policy. If you have it, change a kit version in the file to show that updating once updates everyone. One file, one command, everybody matches.


## 58. Recap

Alright, let's wrap it up. Three things. One, a trusted boundary. The microVM, the network proxy, and credentials the agent never holds. That's Contain and Control. Two, it's your choice what goes inside. Agent kits and mixin kits for your harness, your tools, your endpoints. That's Choice. And three, you share it with the team. One environment file in the repo, update once, everyone follows. That's Capacity. Boundary, choice, sharing.


## 59. Get started

If you want to try this out, here's where to go. The Docker Sandboxes docs are the best place to start. There's a page on environment files if you want to set up team sharing. And if you want to build your own kits, check out the spec repo. Thanks so much, everyone. I think we've got a few minutes for questions.

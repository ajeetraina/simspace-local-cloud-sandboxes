<!-- chrome: false -->

<img src="assets/slide-01.webp" alt="Trust the Box, Not the Agent — a trusted boundary for AI agents with what you need inside it, shared across your team" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Hey everyone, thanks for coming. Today is all about Docker Sandboxes. By the end of this session I want you walking out with one clear picture in your head: a box you can trust, that you get to fill with whatever you need, and that your whole team can share. That's it. That's the talk. Everything else is details and demos, and there are plenty of demos, so if something breaks live, you'll get to watch me sweat in real time.

---

<!-- chrome: false -->

<img src="assets/slide-02.webp" alt="Whale, hello there — meet your speaker, Ajeet Singh Raina, Developer Advocate at Docker" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Very quickly, so you know who's talking at you. My name is Ajeet Singh Raina, I'm a Developer Advocate at Docker. I've spent a little over twenty years in this industry — systems integration, testing, consulting, and developer relations. I was a Docker Captain before I joined the company, and I run a developer meetup in Bengaluru that's grown to about seventeen thousand people. I also co-wrote a book called Operational AI with Docker , which is honestly this talk stretched out to a few hundred pages. But you didn't come here for my résumé. You came here because somewhere in the last few weeks, an agent did something on your machine — or on a machine you're responsible for — that made your stomach drop just a little. Let's talk about that feeling, and then let's

---

<!-- chrome: false -->

<img src="assets/slide-03.webp" alt="Agenda — the problem, why not containers, hello sandboxes, and the four C's: Contain, Control, Choice, Capacity" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Here's the plan. We'll start with the problem, because sandboxes only make sense once you feel the pain. Then a quick detour: why not just use containers? Spoiler, I'm going to break out of one live. Then we'll say hello to sandboxes and walk through four pillars: Contain, Control, Choice, and Capacity. If you caught the WeAreDevelopers keynote, these will sound familiar. They're a really nice way to think about the whole thing.

---

<!-- chrome: false -->

<img src="assets/slide-04.webp" alt="The problem: agents that act — we want the productivity; the question is how to get it safely" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: So let's start with the problem. Everybody wants the productivity agents promise. Nobody wants to be the person whose agent did something weird on a Friday afternoon. How do we get the first thing without the second?

---

<!-- chrome: false -->

<img src="assets/slide-05.webp" alt="Agents need access to do real work — read and write files, run code, install packages, reach the network, use credentials, act on systems" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Agents earn their keep by acting: reading code, running builds, installing packages, calling APIs. Freedom is the feature.

---

<!-- chrome: false -->

<img src="assets/slide-06.webp" alt="More access means a bigger blast radius — on the developer host the agent can read SSH keys, steal cloud credentials, leak .env tokens, control the Docker socket, and exfiltrate over the open internet" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Same agent, same mistake, very different outcome depending on what it can reach. Developers want agents; security teams want control; today they cannot have both.

---

<!-- chrome: false -->

<img src="assets/slide-07.webp" alt="Coding agents act on their own — plan, act, observe, repeat, with no human in the loop; and what happens when the loop goes wrong" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Here's what makes an agent an agent. It plans, it acts, it looks at what happened, and it does it again. Over and over. Hundreds of times. And the whole point is that you're not in that loop. That's the magic. It's also the problem. Because when that loop goes sideways, it can delete files outside your project, it can go poking through your keys and tokens, it can send data somewhere you've never heard of, or it can happily install a package that turns out to be malicious. Not because it's evil. Just because it's trying really hard to finish the task.

---

<!-- chrome: false -->

<img src="assets/slide-08.webp" alt="A Product Catalog sample demo — a Catalog Service with Postgres, AWS S3, Kafka, an inventory service and other external services" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

<!-- Credential keys: a gold key pops in on each trust boundary the catalog
     crosses, so the "every box is a secret you have to trust" point lands
     visually. Same overlay as the containerdays securing-the-agentic-stack-slides
     deck, re-tuned to THIS image's box/arrow coordinates (read off a 1600x900
     grid traced on slide-08.webp — the diagram sits ~10-20px off that deck). -->
<svg viewBox="0 0 1600 900" preserveAspectRatio="none" xmlns="http://www.w3.org/2000/svg" style="position:absolute;inset:0;width:100%;height:100%;pointer-events:none;z-index:2">
  <defs>
    <filter id="keyShadow" x="-60%" y="-60%" width="220%" height="220%">
      <feDropShadow dx="0" dy="1.5" stdDeviation="1.5" flood-color="#000" flood-opacity="0.45"/>
    </filter>
    <g id="credKey" filter="url(#keyShadow)">
      <circle cx="-18" cy="0" r="13" fill="none" stroke="#3a2c00" stroke-width="9"/>
      <circle cx="-18" cy="0" r="13" fill="none" stroke="#F4B400" stroke-width="5.5"/>
      <line x1="-7" y1="0" x2="26" y2="0" stroke="#3a2c00" stroke-width="12" stroke-linecap="round"/>
      <line x1="-7" y1="0" x2="26" y2="0" stroke="#F4B400" stroke-width="7" stroke-linecap="round"/>
      <line x1="15" y1="0" x2="15" y2="10" stroke="#F4B400" stroke-width="6" stroke-linecap="round"/>
      <line x1="24" y1="0" x2="24" y2="12" stroke="#F4B400" stroke-width="6" stroke-linecap="round"/>
    </g>
  </defs>
  <style>
    .credLabel { font-family: Inter, Helvetica, Arial, sans-serif; font-size: 21px; font-weight: 800;
      fill: #c0141c; stroke: #fff; stroke-width: 4px; paint-order: stroke; text-anchor: middle; }
  </style>

  <!-- Database password : Application -> PostgreSQL -->
  <g transform="translate(765,382)">
    <g opacity="0">
      <animate attributeName="opacity" from="0" to="1" begin="0.2s" dur="0.3s" fill="freeze"/>
      <g>
        <animateTransform attributeName="transform" type="scale" from="0.2" to="0.7" begin="0.2s" dur="0.4s" fill="freeze"/>
        <g>
          <animateTransform attributeName="transform" type="scale" values="1;1.15;1" dur="1.8s" begin="0.6s" repeatCount="indefinite"/>
          <use href="#credKey"/>
        </g>
      </g>
      <text class="credLabel" y="44">DB password</text>
    </g>
  </g>

  <!-- AWS keys : Application -> AWS S3 -->
  <g transform="translate(765,488)">
    <g opacity="0">
      <animate attributeName="opacity" from="0" to="1" begin="0.7s" dur="0.3s" fill="freeze"/>
      <g>
        <animateTransform attributeName="transform" type="scale" from="0.2" to="0.7" begin="0.7s" dur="0.4s" fill="freeze"/>
        <g>
          <animateTransform attributeName="transform" type="scale" values="1;1.15;1" dur="1.8s" begin="1.1s" repeatCount="indefinite"/>
          <use href="#credKey"/>
        </g>
      </g>
      <text class="credLabel" y="44">AWS keys</text>
    </g>
  </g>

  <!-- Stripe key : Kafka -> Other service(s) -->
  <g transform="translate(920,600)">
    <g opacity="0">
      <animate attributeName="opacity" from="0" to="1" begin="1.2s" dur="0.3s" fill="freeze"/>
      <g>
        <animateTransform attributeName="transform" type="scale" from="0.2" to="0.7" begin="1.2s" dur="0.4s" fill="freeze"/>
        <g>
          <animateTransform attributeName="transform" type="scale" values="1;1.15;1" dur="1.8s" begin="1.6s" repeatCount="indefinite"/>
          <use href="#credKey"/>
        </g>
      </g>
      <text class="credLabel" y="44">Stripe key</text>
    </g>
  </g>

  <!-- Internal API key : Catalog -> Inventory service -->
  <g transform="translate(1040,300)">
    <g opacity="0">
      <animate attributeName="opacity" from="0" to="1" begin="1.7s" dur="0.3s" fill="freeze"/>
      <g>
        <animateTransform attributeName="transform" type="scale" from="0.2" to="0.7" begin="1.7s" dur="0.4s" fill="freeze"/>
        <g>
          <animateTransform attributeName="transform" type="scale" values="1;1.15;1" dur="1.8s" begin="2.1s" repeatCount="indefinite"/>
          <use href="#credKey"/>
        </g>
      </g>
      <text class="credLabel" y="-30">internal API key</text>
    </g>
  </g>
</svg>

Note: So this is the app we'll be securing all the way down that dev-to-prod road. It's a Product Catalog service, and it's deliberately realistic rather than a toy. In the middle you've got the catalog-service application, and it's not living alone: it writes product data to PostgreSQL, pushes product images to AWS S3, and publishes product updates through Kafka. Reaching outside its own boundary, it talks to an Inventory service and other downstream services. The point I want you to take away is that this is a real supply chain of moving parts - every one of those boxes is something we eventually have to trust and prove. Before we talk about how to fix any of it, I want you to feel it yourself - so let's try.

---

<!-- chrome: false -->

<img src="assets/slide-09.webp" alt="The traditional workflow — a human writes, reviews and ships at every stage of the inner and outer loop; the attack surface is only what you choose to pull" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

<!-- Traditional-workflow motion: the blue twin of the agentic slide that
     follows. Overlays the baked slide-09.webp (people drawn in, same inner/outer
     loop map as slide 10) — dashed rings rotate, a Push dot crosses, a ring
     pulses at each human stage, and two people orbit the loops. Blue (#1d63ed,
     humans) vs the next slide's green (#26ae60, agents). Coordinates match
     slide-10's geometry and the baked person positions (grid-verified). -->
<svg data-anim="slide-09" viewBox="0 0 1600 900" preserveAspectRatio="xMidYMid meet" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;pointer-events:none">
  <defs>
    <radialGradient id="halo09" cx="50%" cy="50%" r="50%">
      <stop offset="0%" stop-color="#1d63ed" stop-opacity="0.55"/>
      <stop offset="55%" stop-color="#1d63ed" stop-opacity="0.16"/>
      <stop offset="100%" stop-color="#1d63ed" stop-opacity="0"/>
    </radialGradient>
    <path id="innerOrbit09" d="M 555,380 A 160,160 0 1,1 555,700 A 160,160 0 1,1 555,380" fill="none"/>
    <path id="outerOrbit09" d="M 1088,325 A 210,210 0 1,1 1088,745 A 210,210 0 1,1 1088,325" fill="none"/>
    <g id="person09">
      <circle cx="0" cy="0" r="27" fill="url(#halo09)"/>
      <circle cx="0" cy="-13" r="8.5" fill="#193363"/>
      <path d="M -16,17 a 16,19 0 0 1 32,0 Z" fill="#193363"/>
    </g>
  </defs>
  <circle cx="555" cy="540" r="160" fill="none" stroke="#193363" stroke-opacity="0.12" stroke-width="2" stroke-dasharray="2 14" stroke-linecap="round"><animateTransform attributeName="transform" type="rotate" from="0 555 540" to="360 555 540" dur="26s" repeatCount="indefinite"/></circle>
  <circle cx="1088" cy="535" r="210" fill="none" stroke="#193363" stroke-opacity="0.12" stroke-width="2" stroke-dasharray="2 14" stroke-linecap="round"><animateTransform attributeName="transform" type="rotate" from="0 1088 535" to="-360 1088 535" dur="34s" repeatCount="indefinite"/></circle>
  <circle r="9" fill="#1d63ed"><animate attributeName="opacity" values="0;1;1;0" keyTimes="0;0.15;0.85;1" dur="2.2s" repeatCount="indefinite"/><animateMotion dur="2.2s" repeatCount="indefinite" path="M 709,540 L 876,540"/></circle>
  <circle cx="560" cy="349" r="22" fill="none" stroke="#1d63ed" stroke-width="3"><animate attributeName="r" values="22;58" dur="2.6s" begin="0.0s" repeatCount="indefinite" calcMode="spline" keySplines="0.3 0 0.3 1" keyTimes="0;1"/><animate attributeName="opacity" values="0.55;0" dur="2.6s" begin="0.0s" repeatCount="indefinite"/></circle>
  <circle cx="373" cy="537" r="22" fill="none" stroke="#1d63ed" stroke-width="3"><animate attributeName="r" values="22;58" dur="2.6s" begin="0.37s" repeatCount="indefinite" calcMode="spline" keySplines="0.3 0 0.3 1" keyTimes="0;1"/><animate attributeName="opacity" values="0.55;0" dur="2.6s" begin="0.37s" repeatCount="indefinite"/></circle>
  <circle cx="429" cy="671" r="22" fill="none" stroke="#1d63ed" stroke-width="3"><animate attributeName="r" values="22;58" dur="2.6s" begin="0.74s" repeatCount="indefinite" calcMode="spline" keySplines="0.3 0 0.3 1" keyTimes="0;1"/><animate attributeName="opacity" values="0.55;0" dur="2.6s" begin="0.74s" repeatCount="indefinite"/></circle>
  <circle cx="696" cy="671" r="22" fill="none" stroke="#1d63ed" stroke-width="3"><animate attributeName="r" values="22;58" dur="2.6s" begin="1.11s" repeatCount="indefinite" calcMode="spline" keySplines="0.3 0 0.3 1" keyTimes="0;1"/><animate attributeName="opacity" values="0.55;0" dur="2.6s" begin="1.11s" repeatCount="indefinite"/></circle>
  <circle cx="1088" cy="301" r="22" fill="none" stroke="#1d63ed" stroke-width="3"><animate attributeName="r" values="22;58" dur="2.6s" begin="1.48s" repeatCount="indefinite" calcMode="spline" keySplines="0.3 0 0.3 1" keyTimes="0;1"/><animate attributeName="opacity" values="0.55;0" dur="2.6s" begin="1.48s" repeatCount="indefinite"/></circle>
  <circle cx="1321" cy="537" r="22" fill="none" stroke="#1d63ed" stroke-width="3"><animate attributeName="r" values="22;58" dur="2.6s" begin="1.85s" repeatCount="indefinite" calcMode="spline" keySplines="0.3 0 0.3 1" keyTimes="0;1"/><animate attributeName="opacity" values="0.55;0" dur="2.6s" begin="1.85s" repeatCount="indefinite"/></circle>
  <circle cx="1088" cy="769" r="22" fill="none" stroke="#1d63ed" stroke-width="3"><animate attributeName="r" values="22;58" dur="2.6s" begin="2.22s" repeatCount="indefinite" calcMode="spline" keySplines="0.3 0 0.3 1" keyTimes="0;1"/><animate attributeName="opacity" values="0.55;0" dur="2.6s" begin="2.22s" repeatCount="indefinite"/></circle>
  <use href="#person09"><animateMotion dur="7s" repeatCount="indefinite" rotate="0" calcMode="linear"><mpath xlink:href="#innerOrbit09" href="#innerOrbit09"/></animateMotion></use>
  <use href="#person09"><animateMotion dur="9.5s" repeatCount="indefinite" rotate="0" calcMode="linear"><mpath xlink:href="#outerOrbit09" href="#outerOrbit09"/></animateMotion></use>
</svg>

Note: Here's the world we grew up in - the traditional workflow. A human writes, reviews, and ships at every stage, and the key line is at the top: the attack surface is only what you choose to pull. You've got the inner loop on the left - code, open source, build, test - the fast cycle on your own machine, and every icon around it is a person. Then you push, and it flows into the outer loop on the right - integrate, test, deploy - again, humans at every point. It's a chain of deliberate human decisions from left to right. Hold this picture in your head, because the next slide keeps the exact same map and swaps out just one thing.

---

<!-- chrome: false -->

<svg data-anim="slide-10" viewBox="0 0 1600 900" preserveAspectRatio="xMidYMid meet" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;"><defs><radialGradient id="halo10" cx="50%" cy="50%" r="50%"><stop offset="0%" stop-color="#26ae60" stop-opacity="0.55"/><stop offset="55%" stop-color="#26ae60" stop-opacity="0.16"/><stop offset="100%" stop-color="#26ae60" stop-opacity="0"/></radialGradient><marker id="arrow10" markerWidth="12" markerHeight="12" refX="8" refY="5" orient="auto"><path d="M0,0 L10,5 L0,10 Z" fill="#193363"/></marker><path id="innerOrbit10" d="M 555,380 A 160,160 0 1,1 555,700 A 160,160 0 1,1 555,380" fill="none"/><path id="outerOrbit10" d="M 1088,325 A 210,210 0 1,1 1088,745 A 210,210 0 1,1 1088,325" fill="none"/></defs><rect x="0" y="0" width="1600" height="900" fill="#ffffff"/><text x="95" y="150" font-family="'Inter Variable', Inter, Helvetica, Arial, sans-serif" font-size="64" font-weight="800" fill="#0a0c10">The Agentic Workflow</text><text x="97" y="222" font-family="'Inter Variable', Inter, Helvetica, Arial, sans-serif" font-size="33" fill="#585858">Now an agent sits at <tspan font-weight="800">every</tspan> stage &#8212; the attack surface is no longer just</text><text x="97" y="266" font-family="'Inter Variable', Inter, Helvetica, Arial, sans-serif" font-size="33" fill="#585858">what you pull.</text><circle cx="555" cy="540" r="160" fill="none" stroke="#193363" stroke-opacity="0.12" stroke-width="2" stroke-dasharray="2 14" stroke-linecap="round"><animateTransform attributeName="transform" type="rotate" from="0 555 540" to="360 555 540" dur="26s" repeatCount="indefinite"/></circle><circle cx="1088" cy="535" r="210" fill="none" stroke="#193363" stroke-opacity="0.12" stroke-width="2" stroke-dasharray="2 14" stroke-linecap="round"><animateTransform attributeName="transform" type="rotate" from="0 1088 535" to="-360 1088 535" dur="34s" repeatCount="indefinite"/></circle><circle cx="555" cy="540" r="140" fill="#ffffff" stroke="#193363" stroke-width="7"/><circle cx="1088" cy="535" r="188" fill="#ffffff" stroke="#193363" stroke-width="7"/><text x="555" y="526" font-family="'Inter Variable', Inter, Helvetica, Arial, sans-serif" font-size="40" fill="#0a0c10" text-anchor="middle">Inner</text><text x="555" y="570" font-family="'Inter Variable', Inter, Helvetica, Arial, sans-serif" font-size="40" fill="#0a0c10" text-anchor="middle">Loop</text><text x="1088" y="521" font-family="'Inter Variable', Inter, Helvetica, Arial, sans-serif" font-size="44" fill="#0a0c10" text-anchor="middle">Outer</text><text x="1088" y="567" font-family="'Inter Variable', Inter, Helvetica, Arial, sans-serif" font-size="44" fill="#0a0c10" text-anchor="middle">Loop</text><line x1="703" y1="540" x2="880" y2="540" stroke="#193363" stroke-width="6" marker-end="url(#arrow10)"/><text x="792" y="510" font-family="'Inter Variable', Inter, Helvetica, Arial, sans-serif" font-size="36" font-weight="800" fill="#585858" text-anchor="middle">Push</text><circle r="9" fill="#26ae60"><animate attributeName="opacity" values="0;1;1;0" keyTimes="0;0.15;0.85;1" dur="2.2s" repeatCount="indefinite"/><animateMotion dur="2.2s" repeatCount="indefinite" path="M 709,540 L 876,540"/></circle><circle cx="560" cy="349" r="22" fill="none" stroke="#26ae60" stroke-width="3"><animate attributeName="r" values="22;58" dur="2.6s" begin="0.0s" repeatCount="indefinite" calcMode="spline" keySplines="0.3 0 0.3 1" keyTimes="0;1"/><animate attributeName="opacity" values="0.55;0" dur="2.6s" begin="0.0s" repeatCount="indefinite"/></circle><circle cx="373" cy="537" r="22" fill="none" stroke="#26ae60" stroke-width="3"><animate attributeName="r" values="22;58" dur="2.6s" begin="0.37s" repeatCount="indefinite" calcMode="spline" keySplines="0.3 0 0.3 1" keyTimes="0;1"/><animate attributeName="opacity" values="0.55;0" dur="2.6s" begin="0.37s" repeatCount="indefinite"/></circle><circle cx="429" cy="671" r="22" fill="none" stroke="#26ae60" stroke-width="3"><animate attributeName="r" values="22;58" dur="2.6s" begin="0.74s" repeatCount="indefinite" calcMode="spline" keySplines="0.3 0 0.3 1" keyTimes="0;1"/><animate attributeName="opacity" values="0.55;0" dur="2.6s" begin="0.74s" repeatCount="indefinite"/></circle><circle cx="696" cy="671" r="22" fill="none" stroke="#26ae60" stroke-width="3"><animate attributeName="r" values="22;58" dur="2.6s" begin="1.11s" repeatCount="indefinite" calcMode="spline" keySplines="0.3 0 0.3 1" keyTimes="0;1"/><animate attributeName="opacity" values="0.55;0" dur="2.6s" begin="1.11s" repeatCount="indefinite"/></circle><circle cx="1088" cy="301" r="22" fill="none" stroke="#26ae60" stroke-width="3"><animate attributeName="r" values="22;58" dur="2.6s" begin="1.48s" repeatCount="indefinite" calcMode="spline" keySplines="0.3 0 0.3 1" keyTimes="0;1"/><animate attributeName="opacity" values="0.55;0" dur="2.6s" begin="1.48s" repeatCount="indefinite"/></circle><circle cx="1321" cy="537" r="22" fill="none" stroke="#26ae60" stroke-width="3"><animate attributeName="r" values="22;58" dur="2.6s" begin="1.85s" repeatCount="indefinite" calcMode="spline" keySplines="0.3 0 0.3 1" keyTimes="0;1"/><animate attributeName="opacity" values="0.55;0" dur="2.6s" begin="1.85s" repeatCount="indefinite"/></circle><circle cx="1088" cy="769" r="22" fill="none" stroke="#26ae60" stroke-width="3"><animate attributeName="r" values="22;58" dur="2.6s" begin="2.22s" repeatCount="indefinite" calcMode="spline" keySplines="0.3 0 0.3 1" keyTimes="0;1"/><animate attributeName="opacity" values="0.55;0" dur="2.6s" begin="2.22s" repeatCount="indefinite"/></circle><rect x="537.0" y="332.0" width="46.0" height="34.0" rx="6.0" fill="#193363"/><rect x="546.0" y="340.0" width="28.0" height="18.0" rx="3.0" fill="#26ae60"/><circle cx="560.0" cy="322.0" r="7.0" fill="#193363"/><text x="560" y="291" font-family="'Inter Variable', Inter, Helvetica, Arial, sans-serif" font-size="27" font-weight="700" fill="#585858" text-anchor="middle">Build</text><rect x="350.0" y="520.0" width="46.0" height="34.0" rx="6.0" fill="#193363"/><rect x="359.0" y="528.0" width="28.0" height="18.0" rx="3.0" fill="#26ae60"/><circle cx="373.0" cy="510.0" r="7.0" fill="#193363"/><text x="327" y="543" font-family="'Inter Variable', Inter, Helvetica, Arial, sans-serif" font-size="27" font-weight="700" fill="#585858" text-anchor="end">Code</text><rect x="406.0" y="654.0" width="46.0" height="34.0" rx="6.0" fill="#193363"/><rect x="415.0" y="662.0" width="28.0" height="18.0" rx="3.0" fill="#26ae60"/><circle cx="429.0" cy="644.0" r="7.0" fill="#193363"/><text x="429" y="733" font-family="'Inter Variable', Inter, Helvetica, Arial, sans-serif" font-size="27" font-weight="700" fill="#585858" text-anchor="middle">Open Source</text><rect x="673.0" y="654.0" width="46.0" height="34.0" rx="6.0" fill="#193363"/><rect x="682.0" y="662.0" width="28.0" height="18.0" rx="3.0" fill="#26ae60"/><circle cx="696.0" cy="644.0" r="7.0" fill="#193363"/><text x="696" y="733" font-family="'Inter Variable', Inter, Helvetica, Arial, sans-serif" font-size="27" font-weight="700" fill="#585858" text-anchor="middle">Test</text><rect x="1065.0" y="284.0" width="46.0" height="34.0" rx="6.0" fill="#193363"/><rect x="1074.0" y="292.0" width="28.0" height="18.0" rx="3.0" fill="#26ae60"/><circle cx="1088.0" cy="274.0" r="7.0" fill="#193363"/><text x="1088" y="243" font-family="'Inter Variable', Inter, Helvetica, Arial, sans-serif" font-size="27" font-weight="700" fill="#585858" text-anchor="middle">Integrate</text><rect x="1298.0" y="520.0" width="46.0" height="34.0" rx="6.0" fill="#193363"/><rect x="1307.0" y="528.0" width="28.0" height="18.0" rx="3.0" fill="#26ae60"/><circle cx="1321.0" cy="510.0" r="7.0" fill="#193363"/><text x="1367" y="543" font-family="'Inter Variable', Inter, Helvetica, Arial, sans-serif" font-size="27" font-weight="700" fill="#585858" text-anchor="start">Test</text><rect x="1065.0" y="752.0" width="46.0" height="34.0" rx="6.0" fill="#193363"/><rect x="1074.0" y="760.0" width="28.0" height="18.0" rx="3.0" fill="#26ae60"/><circle cx="1088.0" cy="742.0" r="7.0" fill="#193363"/><text x="1088" y="831" font-family="'Inter Variable', Inter, Helvetica, Arial, sans-serif" font-size="27" font-weight="700" fill="#585858" text-anchor="middle">Deploy</text><g><circle cx="0.0" cy="0.0" r="27.2" fill="url(#halo10)"/><rect x="-18.4" y="-13.6" width="36.8" height="27.2" rx="4.8" fill="#193363"/><rect x="-11.2" y="-7.2" width="22.4" height="14.4" rx="2.4" fill="#26ae60"/><circle cx="0.0" cy="-21.6" r="5.6" fill="#193363"/><animateMotion dur="7.0s" repeatCount="indefinite" rotate="0" calcMode="linear"><mpath xlink:href="#innerOrbit10" href="#innerOrbit10"/></animateMotion></g><g><circle cx="0.0" cy="0.0" r="27.2" fill="url(#halo10)"/><rect x="-18.4" y="-13.6" width="36.8" height="27.2" rx="4.8" fill="#193363"/><rect x="-11.2" y="-7.2" width="22.4" height="14.4" rx="2.4" fill="#26ae60"/><circle cx="0.0" cy="-21.6" r="5.6" fill="#193363"/><animateMotion dur="9.5s" repeatCount="indefinite" rotate="0" calcMode="linear"><mpath xlink:href="#outerOrbit10" href="#outerOrbit10"/></animateMotion></g></svg>

Note: Same map, same inner and outer loops, same push in the middle - but look what changed: every human icon is now an AI agent. That's the whole point. In the agentic workflow, an agent sits at every single stage - code, open source, build, test, integrate, deploy - and so the attack surface is no longer just what you pull. It's every autonomous action, every tool call, every credential those agents touch across the entire road from inner loop to production. That's a lot of green robots and a lot of blast radius - the same failure mode as that 2:47 AM commit, now at every stage. Let me make it concrete - here's an agent turned loose on a build with no guardrails at all.

---

<!-- chrome: false -->

<img src="assets/slide-11.webp" alt="Agents expand your attack surface automatically — traditional workflow (manual, reviewed) vs agentic workflow (autonomous, no review): the better the agent, the bigger the blast radius" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Here's why this matters. Traditional attacks had to target a developer making a conscious choice — someone had to decide to pull that package, run that script. Agentic systems automate the choosing. The agent pulls and installs without a human reviewing each step, so there's no pause where a person goes "wait, that looks off." And here's the uncomfortable part: the better the agent gets, the more it does on its own — which means the bigger the blast radius. Capability and risk scale together.

---

<!-- chrome: false -->

<img src="assets/slide-12.webp" alt="Agents expand your attack surface automatically — traditional versus agentic workflow, side by side" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Same idea, side by side. On the left, the traditional workflow: manual, reviewed, a human gate at every step. On the right, the agentic one: autonomous, no review, the agent acting end to end. It's the same work getting done — but every one of those review gates on the left is just gone on the right. That's not a small tweak to the threat model; it's a different threat model. So the rest of this talk is about putting one gate back — in a place the agent can't remove.

---

<!-- chrome: false -->

<img src="assets/slide-13.webp" alt="What agents are already doing in your pipeline — pulling dependencies, generating Dockerfiles, editing infrastructure, triggering builds" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: And this isn't the future. It's already in your pipeline. Agents pull dependencies from registries you never audited. They generate Dockerfiles, often with the first base image they see. They edit infrastructure: Terraform, Helm values, Compose files, IAM policies. And they trigger builds on their own, opening PRs, pushing branches and kicking off pipelines. So, what can go wrong?

---

<!-- chrome: false -->

<img src="assets/slide-14.webp" alt="The ungoverned agent — on your host with full permissions it picks FROM node:20 with 6 High CVEs, 431 packages, no SBOM, root user" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Here's what that looks like with no guardrails. The agent on your host, full permissions, asked to containerize the app. It reaches for the first base image it's ever seen — FROM node:20 — and ships it. Look at the scorecard: six High CVEs, 431 packages it pulled in without asking, no SBOM so nobody can tell what's actually in there, and it's running as root. Nothing here is malicious. The agent did exactly what you asked. It just had no reason to pick anything better, and nothing stopping it. Hold that scorecard in your head — six Highs, 431 packages, root — because we're going to run this exact prompt again later inside a sandbox and compare.

---

<!-- chrome: false -->

<img src="assets/clone-the-repo.webp" alt="Set up — clone the Product Catalog showcase repo, confirm there's no Dockerfile or Compose file, ask the agent to containerize it, then docker scout quickview: N critical and NN high CVEs" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Here's the setup for the demo, so you can follow along. Clone the Product Catalog showcase repo, and notice what's not there: no Dockerfile, no Compose file. Then ask the agent to containerize it for production, and scan whatever it builds with docker scout quickview. On the ungoverned run that scorecard comes back ugly — a stack of Critical and High CVEs. That's our baseline, and it's the number we're going to beat later inside a sandbox.

---

<!-- chrome: false -->

<img src="assets/slide-15.webp" alt="Demo time! — the agent containerising the app" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Demo time. Cue: on the host, no sandbox. Prompt the agent to containerize the Product Catalog. Let it write the Dockerfile and build. Then scan the image and read the scorecard out loud — FROM node:20, the High CVEs, the package count, running as root. Don't fix anything yet. The point is to show the default, ungoverned result so we can beat it later.

---

<!-- chrome: false -->

<img src="assets/slide-16.webp" alt="Developers have to choose between speed and safety — approve every action (safe, slow) versus YOLO mode (fast, risky): &quot;I'm babysitting my agents&quot;" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: So today, developers are stuck picking a side. On one end, you approve every single action. Very safe. Also very slow, and you've basically become a full-time button clicker. On the other end, YOLO mode. Skip all the permissions, let it rip. Super fast, but now the agent has exactly the same access you do. Everything you can touch, it can touch. And that's where you hear people say things like, I'm babysitting my agents. Nobody signed up to be a babysitter. We want the right side's speed with the left side's safety.

---

<!-- chrome: false -->

<img src="assets/slide-17.webp" alt="Why not containers? — containers isolate applications well, but agents need more access than applications do" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Okay. Some of you are already thinking: we're at a Docker talk, just put the agent in a container. Fair! Containers are great at isolating applications. But agents aren't applications. They need a lot more access to be useful. Let me show you what happens.

---

<!-- chrome: false -->

<img src="assets/slide-18.webp" alt="Container vs sandbox: whose kernel? — ordinary containers share the host kernel; a Docker Sandbox gets its own Linux kernel in a microVM, so sudo inside is not admin on your Mac" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Here's the difference that matters most, and it comes down to one question: whose kernel? An ordinary container shares the host's kernel. There's one kernel, and everyone — your Mac, every container — is talking to it. A Docker Sandbox is a microVM: it boots its own Linux kernel, completely separate from yours. Why do you care? Because it changes what "root" means. Root inside a container is a short step from root on your host — same kernel, so a kernel-level escape lands on your machine. Root inside the sandbox is root on a throwaway Linux VM that has no path to your Mac. So the agent can sudo all it likes in there, and "admin" inside the box is simply not admin on your laptop.

---

<!-- chrome: false -->

<img src="assets/slide-19.webp" alt="Why not put the agent in a container and restrict it in a boundary? — the Docker socket mount is the catch: it hands the container back the keys to the host" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: So the obvious move is: put the agent in a container, lock it down, draw a nice boundary around it. And that almost works — right up until the agent needs to build or run containers itself, which coding agents do constantly. To let it do that, you mount the Docker socket in. And that one mount quietly hands the keys back. Anything that can talk to the Docker socket can start a new container with the whole host mounted inside it — so the "restricted" container can reach straight past its own boundary to your filesystem. The boundary's still drawn on the slide; it just doesn't hold. Let me show you how fast that goes.

---

<!-- chrome: false -->

<img src="assets/slide-20.webp" alt="Demo time! — escaping a container" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Demo time. Cue: start a container with the Docker socket mounted — the setup everyone reaches for. From inside it, launch a second container with the host root filesystem bind-mounted. Now read a file off the host — something that was supposed to be outside the boundary. That's the escape: no exploit, no CVE, just the socket doing exactly what it's designed to do. Land the line: the container didn't fail, it was never a boundary in the first place.

---

<!-- chrome: false -->

<img src="assets/slide-21.webp" alt="Each new capability weakens the container's isolation — source code, network access, API keys, the Docker socket; the more useful the agent, the less isolated the container" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: So what just happened? Here's the pattern. Every time you give the agent something useful, you chip away at the box. Mount your source code? Now it can change files on your host. Give it network access? It can reach anything, internal or external. Pass in API keys as environment variables? It can read them and send them wherever it likes. And mount the Docker socket? Well, you just saw that one. Isolation is basically gone. The more useful you make the agent, the less isolated the container actually is. That's not a bug in containers. It's just not what they were built for.

---

<!-- chrome: false -->

<img src="assets/slide-22.webp" alt="Hello, Sandboxes — give the agent everything it needs, inside a boundary it can't cross" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: So we need something different. Something where you can give the agent everything it needs, but it still can't get out. Say hello to Docker Sandboxes.

---

<!-- chrome: false -->

<img src="assets/slide-23.webp" alt="A sandbox is a private machine for your agent — your machine stays out; the agent gets a microVM with its own Docker and a proxy as the only way out" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Here's the mental model, and if you remember one slide, make it this one. Think of a sandbox as a private machine just for your agent. On the outside is your laptop with all your stuff: your SSH keys, your other projects, your own Docker. Inside the sandbox there's a microVM, and that's where the agent lives. It gets your project folder, and only that folder. It even gets its own Docker engine, so it can build and run containers all day without ever touching yours. And when it wants to talk to the internet, there's exactly one way out: through the proxy. Three ideas fall out of this. A trusted boundary. You choose what goes inside. And the whole team can reproduce the same setup.

---

<!-- chrome: false -->

<iframe src="labs/trust-the-box-slides/assets/the-box.html" title="The Box — what the agent can reach" style="position:absolute;inset:0;width:100%;height:100%;border:0;background:#fff"></iframe>

Note: This one's interactive — poke at it while I talk. On the left, "No sandbox": the agent is sitting on your computer with everything you own in reach — your project, your photos, your passwords, your documents. Ask it to do something and watch what it can touch. Now flip to "In a sandbox" and run the same request: it only ever sees the project folder. Same agent, same ask, completely different blast radius. That's Contain, as a toy you can click.

---

<!-- chrome: false -->

<img src="assets/slide-24.webp" alt="Four pillars structure the rest of the session — Contain, Control, Choice, Capacity" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: We'll organize the rest of the talk around four pillars. Contain is the boundary itself: the microVM and the guarded network. Control is where you decide where that boundary sits, and where organizations can layer on governance. Choice is about choosing what goes inside, using kits. And Capacity is about making that same sandbox show up for every developer and every run. The first two give you the trusted boundary, the third is your choice, and the fourth is sharing.

---

<!-- chrome: false -->

<img src="assets/slide-25.webp" alt="A sandbox gives you four things — the wall, what it reaches, what's inside, and sharing it: Contain, Control, Choice, Capacity" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Same four pillars, said in plain English so they stick. Contain is the wall — the part the agent can't push through. Control is what it's allowed to reach through that wall. Choice is what you put inside with it. And Capacity is how you give that exact same box to everyone else. Wall, reach, inside, sharing. If the four C's don't land, those four words will. Let's take them one at a time.

---

<!-- chrome: false -->

<img src="assets/slide-26.webp" alt="Contain — the boundaries that keep the agent's actions inside the sandbox" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Pillar one, Contain. Let's look at the actual boundaries that keep the agent's actions inside the box.

---

<!-- chrome: false -->

<img src="assets/slide-27.webp" alt="Three boundaries surround every agent — the microVM, the network proxy, and credential injection" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: There are three boundaries, and I like to think of them like a building. Number one, the microVM. That's a separate room with its own walls. Its own kernel, its own filesystem, its own Docker. Your host just isn't reachable from in there. Number two, the network proxy. That's the front desk. Every request heading out passes the desk, and the desk checks it against policy. Number three, credential injection. The front desk also holds the keys. The agent never carries them. When a request needs a key, the desk adds it on the way out. So inside the room, the agent can go wild. It's the walls and the desk that keep everybody else safe.

---

<!-- chrome: false -->

<img src="assets/slide-28.webp" alt="Credentials are added outside the sandbox — the agent calls an API with no key, the proxy checks policy and injects the key, and the raw key never enters the sandbox" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Let's zoom into that credential bit, because it's honestly my favorite part. The agent wants to call some API. It sends the request with no key at all. The proxy catches it and asks, is this destination allowed? If not, it stops right there. If it is, the proxy grabs the key that's stored on your host and attaches it. The API answers, and the agent gets the result. But the key itself? Never went into the sandbox. So even if the agent gets tricked into dumping every secret it can find, there's nothing to dump.

---

<!-- chrome: false -->

<img src="assets/slide-29.webp" alt="Demo time! — testing the boundaries" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Demo time — let's poke the three boundaries from inside the sandbox. Cue one, the microVM: try to reach a file on the host, or check the kernel, and show there's nothing of yours in there. Cue two, the proxy: curl something the policy doesn't allow and watch it get refused. Cue three, credential injection: show the agent has no key in its environment, then make a call to an allowed API and watch it succeed anyway — the proxy added the key on the way out. Walls, front desk, keys — all three, live.

---

<!-- chrome: false -->

<img src="assets/slide-30.webp" alt="Control — let the agent work, and decide where the boundary sits" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Pillar two, Control. Contain gives us walls. Control is about deciding where those walls sit, so the agent can actually get work done.

---

<!-- chrome: false -->

<img src="assets/slide-31.webp" alt="Developers decide what the agent can reach — network policy profiles Locked Down, Balanced and Open; every connection logged in sbx policy log" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: As a developer, you pick how tight the network is. There are three profiles. Locked Down is the strictest: only what you explicitly allow. Balanced is the default: the common dev stuff like package registries just works, and everything else is denied. Open lets outbound traffic through but still logs it. On top of any profile, you can add your own allow and deny rules. And because everything goes through the proxy, everything gets logged. Run sbx policy log and you can see exactly what the agent tried to reach. It's weirdly satisfying, and occasionally a little alarming.

---

<!-- chrome: false -->

<iframe src="labs/trust-the-box-slides/assets/the-door.html" title="The Door — one way out, with a guard" style="position:absolute;inset:0;width:100%;height:100%;border:0;background:#fff"></iframe>

Note: And here's the proxy, interactive. The agent's inside the sandbox, and the only way to the internet is through one door with a guard. The guard checks an allow-list — the AI model and GitHub are on it, the sketchy site isn't — and every attempt, allowed or denied, shows up in sbx policy log. The key detail: you author that list from outside the box. The agent is inside; it can't edit its own guard. Try sending it somewhere that's not on the list and watch the door hold.

---

<!-- chrome: false -->

<img src="assets/slide-32.webp" alt="Organizations add governance on top — central policy, audit logs, and approved agents and kits enforced across every sandbox, over the always-on Contain layer" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Now, if you're an organization, you probably want more than every developer doing their own thing. That's where AI Governance comes in. Think of it as layers. At the bottom, Contain is always on. MicroVM, proxy, credential injection, no matter what. In the middle, developers set their local policy. And on top, the organization can set central policy, collect audit logs, and say which agents and kits are approved, enforced across every sandbox. So the security team gets visibility and guardrails, and developers still get to move fast.

---

<!-- chrome: false -->

<img src="assets/slide-33.webp" alt="Live demo — adjusting policy: the agent hits a blocked endpoint, find it in the policy log, allow it and keep working" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Demo time. Cue: have the agent hit an endpoint that's blocked. Show it fail. Pull up the policy log and find the denied request. Allow it, and let the agent carry on.

---

<!-- chrome: false -->

<img src="assets/slide-34.webp" alt="Choice — a restricted sandbox is safe; it also needs your tools, your agent, and your endpoints" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Pillar three, Choice. So we've got a nice locked-down box. Great. But a locked-down box with none of your tools in it isn't very useful. How do you make it work for your team, with your agent, your tools, your endpoints?

---

<!-- chrome: false -->

<img src="assets/slide-35.webp" alt="Security tools usually trade away flexibility — sandboxes keep the boundary fixed and make the inside configurable" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Here's the thing about most security tools. They live on this diagonal. The more secure something is, the less flexible it gets. A locked VM with no network is super secure and super annoying. Run the agent right on your host and you get total flexibility and basically zero security. Containers sit somewhere in the middle and slide down the line as you add capabilities. Sandboxes break out of that line. The security comes from the microVM and the proxy, and those never change. What runs inside is completely up to you. So you don't have to pick.

---

<!-- chrome: false -->

<img src="assets/slide-36.webp" alt="Kits define what runs inside the boundary — one agent kit per sandbox, stack as many mixin kits as needed, shared anywhere" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: The way you fill the box is with kits. There are two flavors. Agent kits are the harness: which coding agent runs and how it starts up. You get one per sandbox. Mixin kits are everything else: tools, credentials, network rules. Stack as many as you want on top of any agent. And kits can come from basically anywhere. Docker Hub, any container registry, a Git repo, or just a folder in your project. Underneath all of it, the microVM and proxy stay exactly the same.

---

<!-- chrome: false -->

<img src="assets/slide-37.webp" alt="Swap the agent, add the tools — pick one agent, add any tools; change either side and the boundary stays the same" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: And this is where it gets fun, because it's really just mix and match. Pick one agent. Claude Code, Codex, Hermes, Pi, whatever your team likes. Then add whatever tools you need: GitHub over SSH, SSH commit signing, your project's dev environment setup, maybe code-server if you want VS Code in the browser. Out comes your sandbox. Don't like the agent? Swap it. The tools stay. Need another tool? Add it. The agent stays. And either way, the boundary doesn't budge.

---

<!-- chrome: false -->

<img src="assets/slide-38.webp" alt="Agents talking to an MCP server — external tool servers the agent calls through one governed gateway" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Quick word on how the agent actually uses outside tools, because this sets up the next few slides. The things we keep calling "tools" — a database, GitHub, Stripe, a browser, a hardened-image lookup — the agent reaches through MCP servers. And the key design choice is that the agent doesn't connect to each of those servers itself. It talks to one governed gateway, and the gateway talks to the servers. One door in, one place to apply policy. Keep that single-door picture in mind; everything in the Choice demos runs through it.

---

<!-- chrome: false -->

<img src="assets/slide-39.webp" alt="Agent with a sandbox — the DHI MCP server serves signed tools only; FROM dhi.io/node gives 0 CVEs, 211 packages, SBOM attached, signed, non-root" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Same prompt as our ungoverned agent — containerize the app — but this time it runs inside a sandbox, with its own daemon and its own network. The one thing that changed: before it writes a single FROM line, it asks the DHI MCP server for a hardened base, and it gets back dhi.io/node. Read the scorecard out loud: 211 packages, and zero across the board — zero critical, zero high, zero medium, zero low. SBOM attached, signed, running non-root. Same agent, same prompt, just a better tool within reach.

---

<!-- chrome: false -->

<img src="assets/slide-40.webp" alt="A governed agent — the MCP gateway and catalog, a network proxy with policy and secrets, and filesystem rules, serving the Product Catalog with a 0-CVE base" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Now zoom out from that one result to the whole governed agent. Three things surround it. On one side, the MCP gateway and its catalog — the single door to every external tool, including the DHI lookup we just used. Underneath, the network proxy carrying policy and secrets, so credentials get added on the way out and never sit inside the box. And around the workspace, filesystem rules that keep the agent to the project folder. That zero-CVE Product Catalog image isn't luck — it's what falls out when all three are in place at once.

---

<!-- chrome: false -->

<img src="assets/slide-41.webp" alt="A governed agent — MCP gateway, network proxy, and filesystem policy around the Product Catalog with a 0-CVE base" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Same picture, stripped to the three pieces that matter: the MCP gateway for tools, the network proxy for policy and secrets, and filesystem policy for the workspace — all wrapped around the Product Catalog with its zero-CVE base. This is the shape of a governed agent. Everything we've built in the Choice section lives in one of those three boxes. Hold that shape, because next we look at how the gateway actually enforces what the agent is allowed to do.

---

<!-- chrome: false -->

<img src="assets/slide-42.webp" alt="The agent talks to one gateway, never to servers directly — mcp-gateway is one governed endpoint; every tool call flows through one chokepoint where policy and audit apply" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: The one idea everything hangs on: the agent never talks to those tool-servers directly. It talks to one gateway, and the gateway talks to the servers. That single chokepoint is where authorization, credentials, and audit happen. Every command in the runbook is really just "set up that gateway path" or "manage what's behind it."

---

<!-- chrome: false -->

<img src="assets/slide-43.webp" alt="Point it at a real gateway, nothing else works — a local gateway versus a hosted control plane; fail-closed by design, policy authored in Docker Hub" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: There are two ways to stand up that gateway. A local gateway you run yourself, right next to the sandbox — great for trying this out on your own machine. Or a hosted control plane, where the policy lives centrally and every sandbox in the org points at it. Either way, the important property is the same: it's fail-closed. If the agent isn't pointed at a real gateway, nothing works — tool calls don't fall back to some open path, they just fail. That's deliberate. You can't accidentally end up ungoverned. And the policy itself isn't buried in config on someone's laptop; it's authored in Docker Hub, so there's one place that decides what's allowed.

---

<!-- chrome: false -->

<img src="assets/slide-44.webp" alt="Default-deny allow-list over (server, tool), authored in Cedar — author once, sync everywhere; calls the org policy doesn't permit are denied and audited" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: And here's what that policy actually says. It's a default-deny allow-list, and the thing being allowed isn't a whole server — it's a specific tool on a specific server. So you're not saying "GitHub is allowed," you're saying "the create-pull-request tool on the GitHub server is allowed, nothing else." It's written in Cedar, the same policy language AWS uses for authorization, so it's a real grammar, not a pile of if-statements. You author it once and it syncs everywhere. And anything the policy doesn't explicitly permit is both denied and audited — so you get the block and the record of the attempt. Allow-list, per tool, written once, enforced everywhere.

---

<!-- chrome: false -->

<img src="assets/slide-45.webp" alt="One feature: agents in a sandbox using external tools safely — four practical questions about MCP servers: what's supported, how the sandbox connects, how you register them, how credentials are managed" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: This is really a runbook — the operations guide for one feature: letting an AI agent running inside a Docker sandbox use MCP servers (external tools — Notion, Stripe, GitHub, a browser, etc.) safely. It answers four practical questions: what tool-servers are supported, how you register them, how the sandbox connects to them, and how you manage their credentials. That's the whole scope.

---

<!-- chrome: false -->

<img src="assets/slide-46.webp" alt="Same agent, same prompt — a 0-CVE base: ungoverned picks FROM node:20 (2C 12H 20M 13L); with the DHI MCP it queries first and picks dhi.io/node:24-debian13 (0 CVEs, signed)" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: This is the payoff slide — the before and after, side by side, same agent, same prompt. On the left, the ungoverned run from earlier: FROM node:20, two Critical, twelve High, twenty Medium, thirteen Low. On the right, the same agent with the DHI MCP server available. Before it writes a single FROM line, it asks the server for a hardened base and gets back dhi.io/node:24-debian13 — zero CVEs, signed, with an SBOM. Nothing about the agent changed. We didn't write a better prompt or add a linter. We just gave it a better tool to reach for, behind the governed gateway, and the result went from forty-seven vulnerabilities to zero. That's the whole argument for Choice in one picture.

---

<!-- chrome: false -->

<img src="assets/slide-47.webp" alt="Demo time! — running kits from Docker Hub" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Demo time — filling the box with kits. Cue: start a sandbox with an agent kit plus a mixin or two pulled straight from Docker Hub — the DHI MCP kit is the one to show here. Point out that the agent came from one kit and the tooling from another, stacked on top, no custom image built. Then re-run the containerize prompt and show the agent reaching for the hardened base through the gateway — the zero-CVE result from the previous slide, live. Land it: same boundary, we just chose what went inside.

---

<!-- chrome: false -->

<img src="assets/slide-48.webp" alt="You can build your own kits — kits follow an open specification; package your own agent or tooling the same way the Hub kits are built" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: And yes, you can build your own. Kits follow an open spec, and the repo has some examples to crib from. If your team has a weird internal tool or a custom agent, you can package it up exactly the same way. But building kits is really its own session, so I'll just leave the link here and move on before I get too excited.

---

<!-- chrome: false -->

<img src="assets/slide-49.webp" alt="Capacity — one developer, one sandbox is a start; teams need the same sandbox everywhere" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Last pillar, Capacity. One developer with one nice sandbox is a great start. But the real payoff is when your whole team, and your automation, gets the exact same sandbox every single time.

---

<!-- chrome: false -->

<img src="assets/slide-50.webp" alt="Scaling up starts with reproducibility — one developer by hand, a team by hand (every setup drifts), a team from one file (every sandbox matches)" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: And the secret to scale is reproducibility. One developer setting things up by hand? Totally fine. Now do that across a team. Somebody's on an old kit, somebody forgot a tool, somebody else has a slightly different setup, and suddenly nothing quite matches. Before you can run lots of sandboxes, you need every sandbox to be the same. And that means describing it once, in a file.

---

<!-- chrome: false -->

<!-- Rebuilt as a native HTML slide: the baked slide-51.webp rendered the
     sbxenv.yaml with every line run together and no indentation. This iframe
     shows the schema correctly indented in a real <pre> block (same pattern as
     the-box.html / the-door.html). -->
<iframe src="labs/trust-the-box-slides/assets/env-file.html" title="An environment file describes the whole sandbox — sbxenv.yaml with agent, workspace, kits and resources" style="position:absolute;inset:0;width:100%;height:100%;border:0;background:#fff"></iframe>

Note: That file is the sandbox environment file, sbxenv.yaml. It's pretty small. You say which agent to run, which folder is the workspace, which kits to pull in, and how much CPU and memory to give it. Notice what's not in here: no network rules. That stuff comes with the kits. Then you run sbx env run, it shows you a plan, you approve it, and you're in. Heads up, this is experimental right now, so the format may still change.

---

<!-- chrome: false -->

<img src="assets/slide-52.webp" alt="Commit it once and the whole team shares one setup — change the file, open a pull request, merge, everyone pulls; update once, every sandbox follows" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: And here's the payoff. That file goes in your repo. Need to update a kit, add a new one, or swap the harness? Change the file. Open a pull request. It gets reviewed like any other code. Merge it. And the next time anyone on the team starts their sandbox, they get the new version. Update once, and every sandbox follows. No more wiki page titled how to set up your agent, last updated two years ago.

---

<!-- chrome: false -->

<img src="assets/slide-53.webp" alt="Cloud Sandboxes run the same setup remotely — local on your machine versus cloud on Docker's infrastructure, from the same environment file; early access" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Quick peek at what's coming. Right now your sandbox runs on your laptop, so you're limited by your laptop's CPU and memory. Cloud Sandboxes take that same environment file and run it on Docker's infrastructure instead, so you can spin up more sandboxes than one machine could ever handle. It's early access and not ready for organization use yet, so I won't spend long here. But notice it's the same file. That's why reproducibility matters so much.

---

<!-- chrome: false -->

<img src="assets/slide-54.webp" alt="Demo time! — sharing a sandbox with a team" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Demo time — the team story. Cue: open the sbxenv.yaml in the repo and walk the four lines — agent, workspace, kits, resources. Run sbx env run, let it print the plan, approve it, and you're inside the exact box we've been building all talk. Then make the point land: this file is committed, so the person next to you runs the same command and gets the same sandbox — same agent, same kits, same policy. If you have it, change a kit version in the file to show that updating once updates everyone. One file, one command, everybody matches.

---

<!-- chrome: false -->

<img src="assets/slide-55.webp" alt="Recap — a trusted boundary (Contain, Control), your choice inside it (Choice), and shared with the team (Capacity)" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: Alright, let's wrap it up. Three things. One, a trusted boundary. The microVM, the network proxy, and credentials the agent never holds. That's Contain and Control. Two, it's your choice what goes inside. Agent kits and mixin kits for your harness, your tools, your endpoints. That's Choice. And three, you share it with the team. One environment file in the repo, update once, everyone follows. That's Capacity. Boundary, choice, sharing.

---

<!-- chrome: false -->

<img src="assets/slide-56.webp" alt="Get started — Docker Sandboxes docs, environment files, and the sandbox kit spec" width="1600" height="900" loading="lazy" decoding="async" style="position:absolute;inset:0;width:100%;height:100%;max-width:none;max-height:none;object-fit:fill" />

Note: If you want to try this out, here's where to go. The Docker Sandboxes docs are the best place to start. There's a page on environment files if you want to set up team sharing. And if you want to build your own kits, check out the spec repo. Thanks so much, everyone. I think we've got a few minutes for questions.

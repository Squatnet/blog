# Blog topics

Working list, scoped. Nothing here is scheduled. Status: published, drafted, scoped, needs scoping.

Publishing rules at the bottom.

---

## Track 1: Running an agent that doesn't fall over

**Published** — Eighty skills was too many (2026-10-19)
Per-channel skill lists and system prompts, and the token saving that came with them.

**Drafted** — An agent that checks the agent (2026-10-26)
Config audit, three-minute drain, graceful restart, canary turn.

**Drafted** — Git is the message bus (2026-11-04)
JSON messages in a repo, filename convention, hash polling, review loop.

**Drafted** — Context doesn't announce itself (2026-11-12)
Two thresholds, handover format, handing work between channels.

### What the agent actually does all day
**Premise:** the unglamorous inventory of a running agent, and what it costs.
**Beats:** the cron jobs and which ones are free; where the tokens actually go; a normal day's spend; the work that happens without being asked; the things it is not allowed to do.
**Needs:** a real day's cost ledger and job list, trimmed to what is safe to publish.

### When an agent should touch production
**Premise:** the line between reading and changing, and how to move it deliberately.
**Beats:** read-only by default and why; the approval gate in practice; blast radius thinking; testing on a lifecycle machine before the live one; what a rollback actually looks like when an agent did the damage.
**Needs:** one worked example, ideally the deploy story.

### Failure modes I have caused myself
**Premise:** post-mortems with no hero.
**Beats:** restarting during a build; a config that was correct when written; an agent auditing its own output and agreeing with itself; the false positive that cost a day.
**Needs:** three or four real incidents, dates removed.

---

## Track 2: The self-hosted stack

**Premise for the track:** one person, one box, a lot of services, and the decisions that keep it maintainable.

### The whole estate on one box
**Premise:** what runs where, and what "one box" really costs.
**Beats:** the service inventory by job (git, web, data, agents, media); why it stayed on one machine; the failure domain and what happens when it reboots; what would make me split it; the parts I would not self-host again.

### Caddy as the front door
**Premise:** every service behind one reverse proxy, with TLS that does not need babysitting.
**Beats:** the config shape (sites, upstreams, headers); automatic certificates; subdomains as the routing unit; why Caddy over nginx here; the gotchas with WebSockets and streaming.

### Forgejo at the centre
**Premise:** git hosting that also runs CI, mirrors to GitHub, and stays out of the way.
**Beats:** repos and the runner; which actions work and which do not (the Pages action that exists only on GitHub, and the two red runs it caused); mirroring to GitHub without publishing twice; why self-hosted git beat the hosted options for this.

### Backups you have actually restored from
**Premise:** the difference between a backup and a restore.
**Beats:** what gets backed up (databases, repos, config, secrets); where it goes and how often; the restore drill and how long it took; what was missing the first time; what is still not covered.
**Needs:** a real restore test, with numbers.

### Monitoring without a SaaS bill
**Premise:** enough alerting to sleep, no dashboard nobody reads.
**Beats:** the checks that matter (disk, certs, ports, jobs that stopped running); alert routing into chat; thresholds that do not cry wolf; what is deliberately not monitored.

### Tailscale instead of a firewall
**Premise:** no exposed admin ports, no VPN appliance.
**Beats:** tailnet as the private network; who can reach what; MagicDNS names; exit nodes; the one public thing and why; what breaks when Tailscale is down.

### Why this isn't Kubernetes
**Premise:** compose and systemd are enough at this size, until they are not.
**Beats:** what the stack actually needs; what k8s would add; the failure modes it would introduce; the size at which the answer changes.

---

## Track 3: ERP as the backbone (Frappe / ERPNext)

**Premise for the track:** using an ERP as the application platform, not just as an ERP. Written abstractly, no client names, no SteadQuiet.

### Doctypes as your data model
**Premise:** define the shape once, get forms, APIs, permissions and history.
**Beats:** a doctype as schema plus behaviour; what you get for free; where the abstraction fights you; when a plain table and an endpoint is the better answer.

### Serving a SPA from the ERP
**Premise:** a modern front end on top of Frappe, without turning the ERP into a CMS.
**Beats:** static build plus API; auth and sessions; CSRF and CORS, which is where the time goes; the deploy path; keeping the front end and the backend in step.

### Stockcheck
**Premise:** live stock in a page that works on a phone in a warehouse.
**Beats:** the query and why it must be cheap; caching versus freshness; read-only by design; what it looks like when the ERP is down; the permissions model.

### Live site updates with an approval gate
**Premise:** content changes that go through draft, review and publish, with an audit trail.
**Beats:** where the draft lives; preview; who can approve; what publication does (build, cache purge, version bump); rollback; why the audit trail is the feature.

### Internal tools people actually use
**Premise:** the difference between a tool that gets used and one that gets bypassed with a spreadsheet.
**Beats:** matching the tool to the existing workflow; speed over features; permissions that match reality; the one screen that decides adoption.

### Multi-tenant mistakes to avoid early
**Premise:** the decisions that are cheap on day one and expensive in year two.
**Beats:** tenant keys in every table; permissions and isolation; per-tenant config; migrations; what I would do differently.

---

## Track 4: Short notes

### A bug that took a day and a paragraph
**Premise:** one bug per post, told plainly.
**Beats:** symptom, wrong theories, the actual cause, the fix, what would have caught it sooner.

### The tool I stopped using
**Premise:** retirement notes.
**Beats:** what it did, why it stopped earning its place, what replaced it, what I miss.

### One config change, three failures
**Premise:** the Forgejo workflow story as a short.
**Beats:** the red build, the wrong assumption, the log that gave it away, the split that fixed it.

---

## Track 5: Games

**Needs scoping in conversation.** Which project, what is shareable, devlog format, whether engine and tooling choices are interesting to anyone outside the project.

---

## Rules for publishing

- Weekly, irregular: different weekday and different time of day each time.
- Drafts get read by Matt before anything is published.
- Client work stays abstract. No names, no internal docs, no message bodies.
- No em dashes, no "it's not X, it's Y", no listicle rhythm.
- Every technical claim gets checked against the machine before it ships.

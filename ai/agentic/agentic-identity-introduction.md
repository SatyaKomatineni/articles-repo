<!-- created: 2026-06-11 | updated: 2026-06-11 | topic: AgentCore | keywords: agentic identity, workload identity, OAuth delegation and token exchange, AgentCore vs Entra Agent ID -->

# Agentic Identity: An Introduction

## Questions This Article Answers

- What does "agentic identity" mean, and why is it suddenly a hot topic?
- How is an agent's identity different from the identity of any process or server running today?
- What is "agent identity" concretely in AI systems — what claims, tokens, and objects make it up?
- How does AWS Bedrock AgentCore Identity solve this problem?
- How does Microsoft Entra Agent ID solve this problem?
- How do these approaches compare to each other?
- What does an over-engineered, maximally security-conscious architecture for agent identity look like?
- What are the practical risks of going too far?
- In a real chat-based agent product (chat program + MCP servers + sessions + skills + tasks), what part of the system *is* "the agent"?
- What's a practical, simplified day-one identity model for that kind of system?
- In which scenarios does an agent need to carry *its own* identity, separate from any user?
- What does identity look like for scheduled or event-triggered agents that run with no human present?

## Why this topic exists

Every enterprise already has non-human identities: service accounts, API keys, IAM roles, managed identities, workload identities for containers and Lambda functions. None of this is new. What's new is the *behavior* of the thing holding the identity.

A traditional service account is attached to a fixed, predictable program. It calls the same three downstream APIs every time, on a schedule, with a static permission set that someone reviews once a year. An AI agent is different: it's given a goal, it reasons about how to achieve it, it decides at runtime which tools to call, which APIs to hit, and in what order. It can spin up sub-agents, call other agents, and chain actions across systems in seconds — often thousands of times a day, with each instance living for minutes and then disappearing.

Industry estimates put non-human identities at roughly 45 times the number of human identities in a typical enterprise, and as high as 144:1 in cloud-native environments. Surveys also show that while the large majority of organizations are already running AI agents in production, only a small fraction have a mature strategy for governing the identities behind them. That gap — widespread deployment, immature governance — is why "agentic identity" has become its own discipline rather than just a footnote in IAM.

## 1. How is this different from the identity of a server or process?

A normal server or background process has identity properties that make traditional IAM tractable:

**It's static and long-lived.** A web server's service account is provisioned once, used continuously, and reviewed periodically. Its permission set rarely changes week to week.

**Its behavior is predictable and code-defined.** What the process *can* do and what it *will* do are basically the same thing — the call graph is fixed at build time. If you audit the code, you know the blast radius.

**One identity, one actor, one purpose.** The service account maps cleanly to one application performing one job.

**Permissions are reviewed on a human timescale.** Annual access reviews, periodic credential rotation, and infrequent role changes are good enough because the thing they're protecting doesn't change its own behavior.

An agentic identity breaks all four of these assumptions:

**Ephemeral and high-cardinality.** An agent identity might be created for a single task and destroyed seconds later, and this can happen thousands of times per day. You can't manage these the way you manage a handful of long-lived service accounts — you need a directory and lifecycle automation, not a spreadsheet.

**Behavior is determined at runtime, not at build time.** What the process *can* do (its granted permissions) and what it *will* do (the actual sequence of tool calls and API requests) are no longer the same thing. The code doesn't define the call graph — the model's reasoning does, influenced by the prompt, the data it reads, and the tools it's been given. This means least privilege has to be enforced by the platform, not inferred from reading source code.

**Multiple identities are layered in a single action.** A single API call made by an agent often needs to carry *three* identities at once: the human user who initiated the request, the agent (or agent instance) that's acting, and sometimes the specific tool or sub-agent doing the work. A server process typically only ever asserts one identity — its own.

**Delegation is the default, not the exception.** An agent acting "as itself" is rare; it's almost always acting *on behalf of* a user, or *on behalf of* another agent that delegated a sub-task to it. Standard service accounts rarely need this delegation chain to be cryptographically provable.

**Trust decisions need to happen continuously, not just at provisioning time.** Because the agent's next action isn't predictable, authorization increasingly has to be evaluated per-tool-call, per-resource, in near real time — closer to how you'd treat a human session with adaptive access policies than how you'd treat a static service principal.

In short: a server's identity answers "which fixed program is this?" An agent's identity has to answer "which program, acting for which user, performing which specific action, with permissions narrow enough for *just this step*, and provable after the fact?"

## 2. What is "agent identity" concretely, in AI systems?

Stripped of vendor branding, an agent identity in 2026-era platforms tends to be made up of the same handful of building blocks:

**A registered identity object.** Both AWS and Microsoft now treat an agent as a first-class identity object in a directory — not just an API key. AWS calls this a *workload identity* inside the AgentCore identity directory; Microsoft calls it an *agent identity* (a special kind of service principal) inside Entra ID. Either way, the agent gets a stable, queryable identity record that persists across deployments, redeploys, and different auth schemes.

**A "blueprint" or template for identity at scale.** Because agents are created dynamically and in volume, platforms provide templates — Microsoft's *agent identity blueprints* — so that thousands of agent instances inherit consistent security policy, rather than each one being configured by hand.

**Delegated authorization via OAuth token exchange.** This is the technical heart of agentic identity. The pattern, standardized in things like RFC 8693 (OAuth 2.0 Token Exchange) and an emerging IETF draft for "on-behalf-of" agent authorization, works roughly like this: the agent presents its own credential plus a token representing the user, and receives back a new token that encodes *both* identities — typically a `sub` claim for the user who delegated authority, and an `act` claim for the agent actually performing the action. Downstream systems can then see, in a single token, "Agent A is acting, on behalf of User C, because User C asked Agent B, which delegated to Agent A."

**Short-lived, scoped credentials per task.** Rather than a long-lived API key, the agent obtains narrowly-scoped, short-lived access tokens — ideally one per task or even per tool call — through a credential broker. AWS AgentCore calls this component the *resource credential provider*; it stores the configuration an agent needs to fetch credentials for downstream resource servers (AWS APIs via IAM roles, third-party SaaS via OAuth2, or other tools via API keys) without the agent ever holding long-term secrets.

**An authorizer that gates invocation.** Before an agent even starts running, something needs to check whether the calling user or service is allowed to invoke that agent at all — AWS calls this the *agent authorizer*.

**Lifecycle and governance hooks.** Agent identities need the same lifecycle machinery as human identities: provisioning, ownership assignment (every agent should be mapped to an accountable human or team), periodic access review, anomaly/risk detection, and deprovisioning — ideally via standard protocols like SCIM rather than bespoke scripts.

Put together, "agent identity" is less a single credential and more a *chain of provable claims*: who the agent is, who it's acting for, what it's allowed to do right now, and for how long.

## 3. How are AgentCore and Entra solving this — and how do they compare?

### AWS Bedrock AgentCore Identity

AgentCore Identity is AWS's purpose-built identity and credential management layer for agents running on Bedrock AgentCore. Its core idea is the **workload identity** as a stable anchor: each agent gets one identity that persists across whatever auth mechanism is actually used underneath — IAM roles for AWS resource access, OAuth2 tokens for external SaaS, or API keys for third-party tools. The agent's *identity* doesn't change even if the *credential type* it's using does.

Three components do the work:

- The **agent identity directory** — a central registry of all agent/workload identities, whether created automatically by AgentCore Runtime/Gateway or manually via CLI/SDK.
- The **agent authorizer** — validates whether a user or service is allowed to invoke a given agent in the first place.
- The **resource credential provider** — brokers short-lived credentials for downstream resources (AWS services, OAuth-protected APIs, API-key-based tools), so the agent's code never has to manage long-lived secrets directly.

This is squarely an AWS-native, infrastructure-layer approach: it slots into IAM and the AWS resource model, and is most natural if your agents and most of the systems they call live in or near AWS.

### Microsoft Entra Agent ID

Entra's approach extends the existing Entra ID (Azure AD) identity fabric — the same system that already governs human users, devices, and applications — to cover agents. It introduces several new object types:

- **Agent identity** — a special kind of service principal representing the agent itself.
- **Agent identity blueprint** (and blueprint principal) — a template that lets thousands of agent instances be created with consistent, centrally-managed security policy, with parent-child relationships back to the blueprint.
- **Agent user** — a subtype of user identity, used when an agent needs to act in a context that specifically requires a "user," such as accessing a mailbox or calendar.

Authorization is built on OAuth flows already familiar from human identity — notably an **on-behalf-of (OBO)** flow, where a user's token is exchanged (using the agent's own client credential — ideally a certificate or federated managed identity, not a shared secret) for a new token that represents the agent acting for that user. Because agent identities live in the same directory as everything else, they automatically inherit Entra's existing governance surface: Conditional Access, risk-based adaptive policies, lifecycle management, and Entra ID Governance reviews. Notably, Entra Agent ID is explicitly designed to also cover *non-Microsoft* agents — third-party agents from AWS Bedrock, n8n, etc. can federate in via the Entra Auth SDK or workload identity federation.

### How they compare

The two are less competitors than they are operating at different layers, and increasingly interoperable:

- **AgentCore Identity** is the runtime/credential-brokering layer close to where the agent actually executes and calls tools — it's about *how an individual agent gets the right short-lived credential for the next API call*.
- **Entra Agent ID** is the directory/governance layer close to where the organization manages *who* every agent is, who owns it, what policies apply, and how it's reviewed over time — it's about *organizational visibility and control across all agents, regardless of where they run*.

In practice, a well-architected enterprise system uses both: AgentCore (or an equivalent runtime identity broker) for in-flight credential issuance and scoping, and Entra (or an equivalent enterprise directory) as the system of record, governance, and policy plane that the runtime layer checks against and reports back to. The common thread across both — and the broader industry, per groups like the Cloud Security Alliance and IETF — is convergence on **OAuth 2.1, token exchange (RFC 8693), and SCIM** as the shared protocol layer, rather than every vendor inventing its own.

## 4. How would you over-architect this for a maximally security-conscious posture?

If you wanted to build the most defensible — and most operationally heavy — version of agentic identity, the pattern that emerges from current zero-trust thinking looks like this:

**Per-task, per-tool-call identity issuance.** Don't give an agent a session-long credential. Issue a brand-new, narrowly-scoped, short-lived token for *each individual tool call*, encoding the specific resource, action, and time window — and let it expire immediately after use. This is the "narrower, shorter, more inspectable" model rather than fewer static permissions.

**Full delegation chains in every token, always.** Every action carries the complete chain: original human user → orchestrating agent → any sub-agents → the specific tool. No "flattening" of identity at any hop, even internal ones, so that any downstream system can independently verify the full provenance of a request.

**Mandatory human-owned mapping for every agent identity, with no orphans.** Every agent identity, including transient sub-agents, is registered in the directory and mapped to an accountable owner before it's allowed to obtain its first credential. Automated deprovisioning sweeps remove identities the moment a task completes.

**Continuous, per-call authorization rather than session-based authorization.** Instead of authorizing a session once, evaluate policy on every tool call against real-time context — what the agent has done so far in this task, anomaly signals, data sensitivity of the target resource, and current risk score for the user/agent pair. This is "agentic zero trust": never trust by default, assume breach, continuously re-verify.

**Separate identity and policy planes from the execution plane.** The agent runtime never holds long-term secrets or makes its own authorization decisions — it always calls out to a separate broker/policy decision point, so a compromised agent can't simply self-grant access.

**Full immutable audit trail at the action level.** Every tool call, every credential issuance, every delegation hop logged with the full identity chain attached, retained for compliance review — effectively treating every agent action like a privileged-access session recording.

**Network-level isolation per agent or per task.** Agents (or even individual task executions) run in isolated network segments or sandboxes with explicit egress allow-lists, so that even if an agent's credentials are scoped correctly, it physically cannot reach unapproved endpoints.

**Independent risk scoring and kill-switch per agent identity.** Real-time behavioral anomaly detection on each agent identity (not just the underlying user), with the ability to revoke a specific agent's tokens instantly without affecting the user or other agents.

### The practical risk of going this far

Each of these controls is individually defensible, and large regulated enterprises (finance, healthcare, defense) may genuinely need most of them. But stacked together, they create real costs worth naming:

**Latency and reliability overhead.** Per-tool-call token issuance and continuous policy evaluation add round-trips to every single agent action. For agents that chain dozens of tool calls per task, this can dominate end-to-end latency and introduce new failure modes (the agent works fine until the policy decision point is slow or unavailable).

**Operational complexity outpaces the threat model for most teams.** The arxiv and Cloud Security Alliance research on this topic both note that the *governance gap* — not having a directory, not having owners assigned, not having lifecycle automation at all — is the actual state of most organizations today. Building a fully zero-trust, per-call, network-isolated architecture before you've solved the basics (a directory of agent identities, owners assigned, short-lived credentials instead of static keys) is solving tomorrow's problem while today's is still open.

**Debuggability suffers.** Extremely fine-grained, ephemeral credentials and isolated network segments make it harder for engineers to reproduce and debug agent behavior — every "why did the agent fail here" investigation now also has to rule out an identity/policy denial as the cause.

**False sense of security from complexity.** A long delegation chain and elaborate policy plane can create the impression of safety while the actual gap — e.g., an agent with a tool that's simply too powerful for its task ("can read all of Salesforce" when it only needs one record) — goes unaddressed. Scoping *what the agent can do* well is more valuable than elaborate machinery for proving *who* did it after the fact.

A reasonable starting posture: get every agent into a real identity directory with an owner, replace static API keys with short-lived OAuth tokens issued per session (not necessarily per call), implement the OBO/delegation pattern for anything acting on a user's behalf, and add per-tool-call scoping and continuous authorization selectively — for the highest-risk tools (financial transactions, data deletion, external communications) rather than uniformly across every agent action.

## 5. Bringing it down to earth: a real chat + MCP + skills system

Everything above is written at the level of vendor frameworks and standards. Most people building real systems today don't have an "agent identity directory" or a token-exchange broker — they have something closer to: a chat program, a model, a bunch of MCP servers, some skills, and a task list. So where does "agentic identity" actually show up in *that*?

### "What is the agent here?"

Take a system like: a chat program where a user talks to a model, the model calls tools across various MCP servers, runs skills, and tracks work via tasks across a session.

There isn't one single "agent" — there are several identity-like *roles* layered on top of each other, and the trick is telling them apart:

**The human user.** This is the person who opened the chat and is ultimately accountable for what happens. Their identity (email, account, OAuth login to each connected service) is the root of authority — almost everything the system does should trace back to "this user asked for this, or authorized this."

**The chat program / harness.** This is the runtime — the thing that holds the conversation, decides which tools are available, calls the model, and executes tool calls. In agentic-identity terms, *this* is closest to the "agent" as a workload: it's the long-lived, deployed thing that has its own service identity (e.g., it might authenticate to MCP servers using its own client credentials, OAuth client ID, or managed identity). One chat program serving many users typically has *one* workload identity, reused across sessions — the way a web server has one service account regardless of how many users hit it.

**The session.** A session is the closest thing to an *ephemeral agent instance*. It's scoped to one user, one conversation, runs for a bounded time, and is the natural unit for "this specific run of the agent, for this user, doing this task." If you were going to assign a short-lived workload identity to anything, the session is the natural candidate — it's the equivalent of AWS AgentCore's "workload identity created per task" or Entra's "agent identity that might exist for minutes."

**Skills and tasks.** These are *capabilities and units of work*, not identities. A skill is more like a function the agent can call — it doesn't need its own identity any more than a library function does. A task is a tracked unit of work within a session. Neither of these typically needs a separate identity in a day-one model; they inherit the session's identity and permissions.

**MCP servers / connectors.** These are the *resources* — the downstream systems being accessed (Slack, Google Drive, a database, etc.). Each one has its own notion of who's calling: an OAuth token, an API key, a service account. This is where "on whose authority is this call being made?" becomes concrete.

So, mapping to the earlier vocabulary:

| Concept from Section 2 | In a chat + MCP system |
|---|---|
| Human user | The logged-in person using the chat app |
| Workload identity (long-lived) | The chat program/harness itself |
| Workload identity (ephemeral, per-task) | A session (or a single agentic run within it) |
| Delegated/OBO token | The OAuth token the chat program uses to call an MCP server *as the user* |
| Resource | Each MCP server / connected tool |
| Skill, task | Capabilities and work units — not identities |

### A practical day-one model

You don't need a directory, a blueprint system, or per-tool-call token issuance to get the *core* benefit of agentic identity thinking. Day one looks like this:

**1. Every MCP/tool connection is made as the user, not as the app, wherever possible.** When the chat program connects to Slack, Google Drive, GitHub, etc., it should use *that user's* OAuth grant — not one shared service-account credential for the whole app. This single decision gets you most of the "delegation" story for free: every action downstream is naturally attributable to the user who authorized it, with the chat program as the visible "actor" (most OAuth apps already show "App X acting as You" in their audit logs — that *is* an `act`/`sub` pair, just without the formal RFC 8693 plumbing).

**2. Treat the session as your unit of "agent identity," informally.** Give each session a unique ID, log it alongside every tool call, and make sure that ID ties back to (a) the user and (b) what the agent was asked to do. You don't need a formal workload-identity object — a well-logged session ID that appears in every downstream audit trail gets you 80% of the traceability value.

**3. Scope OAuth grants per-connector, not globally.** If the chat program asks for Slack access, request the narrowest Slack scopes that the skills you actually use require — not "admin" just in case. This is the cheap version of "least privilege per task": you're not rotating credentials per tool call, but you're also not handing the model a master key.

**4. Keep a simple registry of "what can call what."** Even a markdown table or config file listing each MCP server, what scopes it's granted, and which skills/tasks use it gives you most of the value of an "agent identity directory" without building one. The goal is just: if something goes wrong, you can answer "what did this connection have access to?" in under a minute.

**5. Log the delegation chain in plain text.** For any consequential action (sending a message, writing a file, calling a paid API), log: which user, which session, which skill/task, which MCP server, what action. That's the informal equivalent of the `sub`/`act` claim chain — and it's enough for almost all post-incident review at small-to-medium scale.

**6. Reserve the heavy machinery for the genuinely risky actions.** Most tool calls (reading a file, searching, summarizing) don't need anything beyond the above. For the small set of actions that are hard to undo — sending external messages, deleting data, spending money, modifying access controls — that's where it's worth adding an extra check: a confirmation step, a narrower just-in-time credential, or a human-in-the-loop approval. This mirrors the "selective per-call scoping for high-risk tools" recommendation from Section 4, without applying it everywhere.

### The one-sentence version

*The chat program is the long-lived agent (a workload identity); each session is a short-lived instance of that agent acting for one user; every tool call should be traceable to "this user, via this session, through this connector" — and that traceability, plus narrow OAuth scopes, covers the vast majority of real-world risk without needing a formal identity platform.*

## 6. When does an agent need to carry its own identity?

In the day-one model above, the agent mostly *borrows* the user's identity — it acts "as the user" via OAuth, and the agent's own identity is just metadata (a session ID) attached for traceability. That works as long as a human is present, in the loop, in real time.

That assumption breaks in a specific, recognizable set of scenarios — and those are exactly the cases where "agentic identity" stops being a nice-to-have and becomes load-bearing.

### Scenario 1: Scheduled agents (the "every morning at 6am" case)

If an agent runs on a cron schedule, there's no live user session to borrow a token from. The user who *set up* the schedule isn't sitting at their laptop at 6am. Two sub-patterns exist:

- **Delegated-but-offline.** The agent still acts on behalf of a specific user, but using a credential obtained in advance — typically an OAuth refresh token (`offline_access` scope) captured when the user set up the schedule, exchanged for a fresh access token at run time. The agent's identity here is "this scheduled task, which was authorized by User X on [date], acting as User X."
- **Service identity.** The agent acts under its own dedicated identity (a bot account, service principal, or app-only token) rather than impersonating a specific person — e.g., "the daily-report-bot" that posts to a channel using its own Slack app identity, not any individual's.

Either way, the agent now needs an identity that **persists independently of any session** — because the thing that triggers it (a clock) has no identity of its own.

### Scenario 2: Event/webhook-triggered agents

An agent triggered by a GitHub push, a new support ticket, an inbound email, or a queue message is similar to the scheduled case but the "trigger" carries *some* context — often an external identity (the GitHub user who pushed, the customer who emailed) that is *not* the same as the identity that should be used to act.

Here the agent typically needs **its own service identity** for taking action (commenting on the PR, replying to the ticket), while *recording* the triggering external identity as context/audit data, not as the acting principal. You generally don't want a random external GitHub user's identity to flow through as "who has permission to do this" — that's a privilege-escalation path. The agent's own identity is what's actually authorized.

### Scenario 3: Agent-to-agent calls (multi-agent systems)

When Agent A calls Agent B as a sub-task, Agent B needs to know *which agent* is calling it — separately from which (if any) human user is ultimately behind the chain. This matters for:

- **Throttling/billing** — Agent B may rate-limit or meter differently per calling agent.
- **Authorization** — Agent B might allow Agent A to read data but not write it, regardless of what the end user could do.
- **Audit** — "Agent B did X" is incomplete; you need "Agent B did X because Agent A asked it to, because User C asked Agent A."

This is the scenario the full delegation-chain / `act` claim machinery (Section 2) is really built for — each hop adds itself to the chain rather than the chain collapsing to just "User C did X."

### Scenario 4: Long-running autonomous agents (monitoring, remediation, "always-on")

Think of an agent that watches infrastructure metrics and automatically restarts services, or one that continuously triages an inbox. There's no single "task" with a beginning and end tied to a user request — the agent is more like a *running service*. Its identity needs to behave like a **persistent workload identity**: registered, owned by an accountable team, with permissions reviewed periodically — much closer to a traditional service account, except the platform still needs to track *what it actually did*, since (per Section 1) its actions aren't fully predictable from its code.

### Scenario 5: Cross-organization / B2B agents

When an agent from Company A calls into Company B's systems (e.g., a vendor's support agent querying your ticketing system), Company B needs to authenticate and authorize *Company A's agent* as a distinct principal — separate from any individual human at either company. This is where federated workload identity (the kind of cross-platform federation Entra Agent ID supports for non-Microsoft agents) becomes necessary: B needs to verify "this is a legitimate agent from A, with these specific permissions," independent of any user-level trust relationship.

### Scenario 6: Elevated or sensitive standing permissions

Any agent that holds permissions a typical user wouldn't have — e.g., an agent that can deploy infrastructure, modify IAM policies, or access an entire data warehouse for analytics — needs its own identity *regardless* of whether a user is present. The reasoning is accountability: if the agent's permissions exceed what any single user could grant via delegation, the agent's actions can't be fully explained as "acting on behalf of" someone — the agent itself is a privileged principal that needs its own lifecycle, owner, and review cadence.

### What this looks like in practice for scheduled/event-triggered agents

Bringing this back to something concrete — for a scheduled or event-triggered agent in a chat-program-style system, a workable identity model is:

1. **Give the schedule/trigger its own identity record at creation time.** When a user creates a scheduled task or registers a webhook-triggered agent, create a durable identity for *that schedule/trigger* — not just a config entry. This is the thing that will run unattended.

2. **Capture the authorizing user explicitly, and store it as metadata, not as ambient context.** "User X authorized this scheduled agent on this date, with these scopes" should be a queryable fact — this is your accountable owner, and it's what you revoke if User X leaves the company or is offboarded. Don't let scheduled agents silently keep running on a departed employee's credentials (this is one of the most common real-world incidents in this space).

3. **Use offline-capable delegated tokens (refresh tokens) where the action is genuinely "as the user," and dedicated service/bot identities where it's genuinely "as the system."** Don't default to one or the other — a scheduled report that reads *your* calendar and emails *you* a summary should run as you; a scheduled agent that posts a daily standup summary to a shared channel probably should run as a bot, not as whoever happened to set it up.

4. **Re-validate at run time, not just at creation time.** Before each scheduled run, check that the authorizing user/credential is still valid (not revoked, not offboarded, scopes haven't been pulled). A scheduled agent that keeps running on a stale, never-rechecked credential is exactly the "orphaned non-human identity" problem the industry research keeps flagging.

5. **Log every unattended run with the same delegation-chain detail as an interactive one.** "Ran at 6:00am, as scheduled-task-id Y, authorized by User X as of [date], called connectors A/B/C with scopes S1/S2" — so an unattended run is just as auditable as one a human watched happen.

6. **Periodically review standing schedules and triggers like you'd review standing access grants.** Because nobody is "present" when these run, they're the agentic equivalent of a service account with a never-expiring password — they need to show up in access reviews, not just in a list of configured automations.

The short version: **the moment an agent can act without a human present, it stops being able to borrow an identity and needs one of its own** — either a delegated-but-persisted credential tied to an accountable human owner, or a dedicated service identity with its own lifecycle. Either way, the thing that must never happen is an agent identity that exists in practice but isn't *registered* anywhere as existing.

## Sources

| # | Link | What you'll find there | Value |
|---|------|------------------------|-------|
| 1 | [Exploring IAM for AI Agents in 2026 \| Strata](https://www.strata.io/blog/agentic-identity/why-ai-agents-deserve-first-class-identity-management-7b/) | Framing of why agentic identities differ from static non-human identities, NHI ratio stats | Medium |
| 2 | [A New Identity Playbook for AI Agents in 2026 \| Strata](https://www.strata.io/blog/agentic-identity/new-identity-playbook-ai-agents-not-nhi-8b/) | Industry perspective on standards convergence (OAuth 2.1, MCP, A2A) for agent identity | Medium |
| 3 | [The Non-Human Identity Governance Vacuum – Cloud Security Alliance](https://labs.cloudsecurityalliance.org/research/csa-whitepaper-nonhuman-identity-agentic-ai-governance-v1-cs/) | Whitepaper on the governance gap — adoption vs. NHI strategy maturity | High |
| 4 | [AI Agents at Work 2026: Securing the agentic enterprise \| Okta](https://www.okta.com/newsroom/articles/ai-agents-at-work-2026-agentic-enterprise-security/) | Enterprise adoption survey data and security framing for agentic AI | Medium |
| 5 | [Provide identity and credential management for agent applications with Amazon Bedrock AgentCore Identity \| AWS Docs](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/identity.html) | Official AgentCore Identity overview and capabilities | High |
| 6 | [Understanding workload identities \| Amazon Bedrock AgentCore](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/understanding-agent-identities.html) | How AgentCore workload identities work as stable anchors across credential types | High |
| 7 | [Understanding the agent identity directory \| Amazon Bedrock AgentCore](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/agent-identity-directory.html) | Details on the agent identity directory as the registry for agent/workload identities | High |
| 8 | [Introducing Amazon Bedrock AgentCore Identity: Securing agentic AI at scale \| AWS](https://aws.amazon.com/blogs/machine-learning/introducing-amazon-bedrock-agentcore-identity-securing-agentic-ai-at-scale/) | Launch blog explaining the authorizer, directory, and credential provider components | High |
| 9 | [What is Microsoft Entra Agent ID? \| Microsoft Learn](https://learn.microsoft.com/en-us/entra/agent-id/what-is-microsoft-entra-agent-id) | Official overview of the Entra Agent ID framework and its purpose | High |
| 10 | [Overview of agent identities in Microsoft Entra \| Microsoft Learn](https://learn.microsoft.com/en-us/entra/agent-id/agent-identities) | Definitions of agent identity, blueprint, and agent user object types | High |
| 11 | [What are agent identities? \| Microsoft Entra Agent ID](https://learn.microsoft.com/en-us/entra/agent-id/what-are-agent-identities) | Deeper detail on agent identities as service principals and their lifecycle | High |
| 12 | [Agent OAuth flows - On-behalf-of flow \| Microsoft Entra Agent ID](https://learn.microsoft.com/en-us/entra/agent-id/agent-on-behalf-of-oauth-flow) | Walkthrough of the OBO flow and token exchange for agents acting as users | High |
| 13 | [OAuth 2.0 Extension: On-Behalf-Of User Authorization for AI Agents (IETF draft)](https://www.ietf.org/archive/id/draft-oauth-ai-agents-on-behalf-of-user-01.html) | Draft standard introducing `requested_actor`/`actor_token` for agent delegation | High |
| 14 | [RFC 8693 - OAuth 2.0 Token Exchange](https://datatracker.ietf.org/doc/html/rfc8693) | The base spec for token exchange and delegation/impersonation semantics (`act` claim) | High |
| 15 | [Explaining OAuth Delegation, 'On Behalf Of', and Agent Identity for AI Agents \| Christian Posta](https://blog.christianposta.com/explaining-on-behalf-of-for-ai-agents/) | Accessible explanation of delegation vs. impersonation for agents | Medium |
| 16 | [Why do AI agents complicate zero trust and least privilege models? \| NHIMG](https://nhimg.org/faq/why-do-ai-agents-complicate-zero-trust-and-least-privilege-models/) | Explanation of why static least-privilege roles break down for adaptive agents | Medium |
| 17 | [Identity Management for Agentic AI (arXiv)](https://arxiv.org/pdf/2510.25819) | Academic survey of authentication/authorization approaches for agentic AI | High |
| 18 | [Zero Trust for autonomous agentic AI systems \| Red Hat](https://next.redhat.com/2026/02/26/zero-trust-for-autonomous-agentic-ai-systems-building-more-secure-foundations/) | "Agentic Zero Trust" framing — never trust by default, shrink blast radius | Medium |

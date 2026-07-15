<!-- created: 2026-07-15 | updated: 2026-07-15 | topic: MCP | keywords: MCP naming conventions, tool naming, semantic verb vocabulary, server naming, AgentCore Gateway target naming -->

# Naming Standards for Enterprise MCP Implementations

*Version 1.0 · Status: Draft · Last updated: 2026-07-15 · Researcher: Satya Komatineni*

Naming standards for enterprise MCP implementations, covering **servers**, **tools**, and **server endpoints** across both **on-prem / self-hosted** servers and **AWS Bedrock AgentCore Gateway**. For platform engineers, MCP server developers, and architects setting org-wide conventions, grounded in the hard constraints of the MCP spec and AWS AgentCore. Specifically, these are the questions it answers:

## Questions This Article Answers

1. What are the hard character/length rules the MCP spec imposes on tool names?
2. What naming pattern should tools follow, and why does `domain_noun_verb` win?
3. What is the controlled verb vocabulary for reads, writes, searches, updates, and deletes?
4. How should MCP servers themselves be named, and where does reverse-DNS namespacing fit?
5. How do MCP "endpoints" (transport URLs, resource URIs) get named?
6. What extra naming rules does AWS AgentCore Gateway impose on targets and tools?
7. Why does the AgentCore `target___tool` prefix change how you must budget tool-name length?
8. What prefixes and suffixes are appropriate at each layer (env, domain, version, read/write)?
9. How do on-prem naming choices differ from AgentCore-hosted ones?
10. What are the common naming mistakes that cause collisions or broken policies?
11. Which entities need names, and what is the recommended pattern for each?
12. What is a minimal quick-start ruleset a team can adopt today?

## 1. Why Naming Is a First-Class Concern in MCP

Naming in MCP is not cosmetic. The tool name is part of the **prompt surface** the model reasons over: the LLM chooses tools largely from their names and descriptions, so a clear, consistent name directly improves tool selection accuracy. Names are also **identity** — they appear in authorization policies, audit logs, and registries, and once published they are effectively an API contract that agents depend on. Poorly chosen names cause three concrete failures: model confusion (the agent calls the wrong tool), name collisions (two servers expose `search`, and the client cannot disambiguate), and broken governance (a renamed tool silently breaks a Cedar/IAM policy that referenced the old name).

Three forces constrain any convention you pick:

1. **The MCP spec's hard limits** on the tool-name character set and length.
2. **Client-side namespacing** — many MCP clients prepend the server name to each tool (e.g. `mcp__github__issue_create`), so the effective name the model sees is longer than what you declared.
3. **Host-platform rules** — AgentCore Gateway rewrites every tool name to `target___tool`, which both consumes length budget and forbids certain characters in the target segment.

## 2. Hard Constraints You Cannot Violate

These are non-negotiable, protocol- and platform-level rules. Conventions live *inside* these limits.

1. **MCP tool-name regex:** `^[a-zA-Z0-9_-]{1,64}$`. Only letters, digits, underscore, and hyphen; **no dots, spaces, slashes, or colons**; length **1–64 characters**.
2. **snake_case is the de facto standard.** Over 90% of published tools use `snake_case`; it also tokenizes best for the models doing the selecting. Prefer it over camelCase or kebab-case for tool names.
3. **AgentCore Gateway target-name regex:** `([0-9a-zA-Z][-]?){1,100}` — alphanumeric plus hyphens only, **1–100 chars, no underscores** in the target name.
4. **AgentCore exposed tool name = `${target_name}___${tool_name}`** (three underscores). The prefix is permanent and visible to every client; your Lambda/handler must strip it back off.
5. **AgentCore gateway identifier** carries a system-appended `-{10-char}` suffix (`([0-9a-z][-]?){1,48}-[0-9a-z]{10}`), so the human-chosen portion of a gateway name is effectively ≤48 lowercase chars.
6. **MCP registry server names use reverse-DNS namespaces** — e.g. `io.github.<org>/<server>` or `com.<company>/<server>` — to guarantee global uniqueness and provenance.
7. **Budget the length.** Because AgentCore prepends `target___`, an underlying tool named up to 64 chars can exceed a downstream consumer's comfort once the target prefix is added. Keep underlying tool names short (aim ≤ ~40 chars) when the server will sit behind a Gateway.

## 3. Naming Tools

Tools are the highest-leverage names because the model reads them directly.

1. **Pattern: `domain_noun_verb`** (AWS Prescriptive Guidance's recommended standard). Examples: `claims_claim_get`, `claims_claim_submit`, `claims_appeal_create`, `member_eligibility_check`. The noun acts as an organizing boundary so that alphabetical listings cluster related operations together, which helps both the LLM scan and human documentation.
2. **Lead with a domain prefix** when a server exposes many tools or when tools from several servers share a client — `github_issue_create` will never collide with `gitlab_issue_create`. The prefix is the cheapest collision-avoidance mechanism available.
3. **Use verbs from a controlled vocabulary.** Standardize on a small set — `get`, `list`, `search`, `create`, `update`, `delete`, `submit`, `check` — rather than letting each team invent `fetch` vs `retrieve` vs `read`. Consistency is worth more than expressiveness.
4. **Encode side-effect risk in the verb, not a suffix.** `_get`/`_list`/`_search` are read-only; `_create`/`_update`/`_delete` mutate. This lets agents and guardrails filter by intent. If you must flag destructive tools explicitly, a `_danger` or `_admin` suffix is acceptable, but prefer separating read and write tools into different servers.
5. **Almost always multi-word.** Single-word tools (`search`, `run`) are collision magnets and give the model too little signal. ~95% of real tool names are multi-word — follow suit.
6. **Do not put version numbers in the tool name by default.** Version the server/endpoint, not the tool (see §6). Only pin a version into a tool name (`_v2`) when two incompatible versions must coexist in the same server simultaneously.
7. **Keep it short behind a Gateway.** Under AgentCore the client sees `target___tool`, so favor `claim_get` over `claims_processing_claim_record_get` when the target already carries the domain.

## 4. Semantic Verb Vocabulary (Reads, Writes, Searches, Updates, Deletes)

The verb is the most important token in a tool name because it is what agents and guardrails use to reason about **side-effect risk** without understanding the tool. Standardize on a **closed, controlled verb set** where the verb alone tells you the safety class (read / write / destructive), rough idempotency, and the HTTP semantics. Group the vocabulary into four classes.

### 4.1 Read family — safe, no side effects

1. `get_` — fetch **one** known entity by id. `claims_claim_get`
2. `list_` — enumerate a collection with no query (or simple paging). `claims_claim_list`
3. `search_` — filtered/predicate query returning matches. Reserve `find_`/`query_` as synonyms you deliberately do **not** also use — pick one. `member_eligibility_search`
4. `describe_` — return schema/metadata *about* an entity or capability, not its data. `claims_schema_describe`
5. `check_` / `validate_` — evaluate and return a status/boolean, read-only. `member_eligibility_check`
6. `export_` — bulk read-out. `claims_claim_export`

### 4.2 Write family — mutating, non-destructive

1. `create_` — make a new entity; typically **not** idempotent (POST-like). `claims_appeal_create`
2. `update_` — partial modify of an existing entity (PATCH-like). `claims_claim_update`
3. `set_` / `replace_` — full replace; idempotent (PUT-like). `member_profile_set`
4. `upsert_` — create-or-update. `member_profile_upsert`
5. `submit_` — kick off a process/workflow (a state transition, not just data). `claims_claim_submit`
6. `cancel_` — reverse/withdraw a submitted process. `claims_claim_cancel`

### 4.3 Destructive family — irreversible or data-losing

1. `delete_` — remove an entity (soft or hard). `claims_draft_delete`
2. `purge_` / `archive_` — hard-delete / retire variants, only when you must distinguish them from a soft `delete_`.

### 4.4 Action / RPC family — side-effecting, not CRUD

1. `send_`, `trigger_`, `run_`, `sync_`, `approve_`, `assign_` — genuine verbs for genuine actions. Don't force these into CRUD words.

### 4.5 The rules that make the vocabulary work

1. **One verb per meaning.** The single most important rule. Never let `get` / `fetch` / `retrieve` / `read` coexist — the model can't tell them apart and neither can a reviewer. Publish the closed list and lint against it.
2. **Side-effect class must be inferable from the verb.** `get/list/search/describe/check/export` = read; everything else = write; `delete/purge/archive` = destructive. This is what lets you (a) run a **read-only server** whose whole tool set is safe by construction, (b) apply **human-in-the-loop** only to the destructive class, and (c) filter tools by user permission.
3. **Idempotency follows the verb.** `get/list/search/set/delete` are idempotent; `create/submit` are not. A consistent mapping means agents can safely retry the idempotent ones.
4. **Cardinality lives in the verb, not the noun.** Keep the noun stable and let `get_` (one) vs `list_`/`search_` (many) carry singular-vs-collection — `claims_claim_get` and `claims_claim_list`, not `claim_get` vs `claims_list`.
5. **Placement — prefer verb-*last* for enterprise servers.** `noun_verb` (`claim_get`, `claim_update`, `claim_delete`) makes alphabetical listings cluster every operation on a resource together, per AWS's `domain_noun_verb` standard. Verb-*first* prefixes (`get_claim`, `delete_claim`) cluster by operation instead — choose that only if your primary axis really is "show me all deletes."

### 4.6 Verb quick reference

| Verb | Class | HTTP analog | Idempotent | Needs HITL? |
|------|-------|-------------|-----------|-------------|
| `get` / `list` / `search` / `describe` / `check` / `export` | Read | GET | Yes | No |
| `create` / `submit` | Write | POST | No | Maybe |
| `update` | Write | PATCH | No | Maybe |
| `set` / `replace` / `upsert` | Write | PUT | Yes | Maybe |
| `delete` / `purge` / `archive` | Destructive | DELETE | Yes | **Yes** |
| `send` / `trigger` / `run` / `approve` / `assign` | Action | POST | Varies | Often |

## 5. Naming Servers

The server name is the namespace for its tools and the unit of trust in a registry.

1. **On-prem / internal registry: use reverse-DNS namespacing.** Format `com.<company>.<domain>/<server-name>`, e.g. `com.acme.claims/claims-core`, `com.acme.member/eligibility`. This matches the official MCP registry convention, guarantees uniqueness, and encodes ownership/provenance.
2. **The short `<server-name>` segment should be kebab-case, domain-first, and describe the capability, not the technology** — `claims-core`, `member-eligibility`, `care-scheduling`, not `mcp-server-1` or `springboot-api-wrapper`.
3. **Encode environment where the platform doesn't already isolate it.** Prefer environment separation via namespace or deployment (separate registries/accounts per env). If you must encode it in the name, use a consistent suffix: `-dev`, `-stg`, `-prod` — e.g. `com.acme.claims/claims-core-prod`.
4. **Split by separation of duties, and let the name say so.** If you separate read from write servers, name them `claims-read` and `claims-write`. Cap any single server at **~50 tools**; beyond that, split by noun/sub-domain and give each split a distinct server name.
5. **The client-visible prefix is derived from the server name.** Many clients present tools as `mcp__<server>__<tool>` or `<server>.<tool>`, so a clean, short server name keeps the fully-qualified tool readable.

## 6. Naming Server Endpoints

"Endpoint" spans two distinct things in MCP — the transport URL and the resource URIs a server exposes.

1. **Transport URL path (Streamable HTTP):** standardize on a single, predictable path per server. Convention: `https://<host>/mcp/<server-name>` (e.g. `https://mcp.acme.internal/mcp/claims-core`). Keep the literal `/mcp` segment so gateways, WAFs, and logs can identify MCP traffic uniformly.
2. **Version the endpoint, not the tool.** Put the major version in the path — `https://mcp.acme.internal/mcp/claims-core/v1` — so breaking changes get a new path while tool names stay stable. This is the primary place versioning belongs.
3. **Host naming:** a dedicated subdomain such as `mcp.<company>.<env>` or `mcp-<domain>.<company>` keeps MCP endpoints discoverable and easy to firewall as a class.
4. **Resource URIs** exposed by a server should use a consistent, meaningful scheme and hierarchy: `<scheme>://<domain>/<type>/<id>`, e.g. `claims://claim/12345`, `member://eligibility/9876`. Keep the scheme aligned with the server's domain so resources are self-describing.
5. **Don't overload one endpoint with unrelated domains.** One server = one coherent domain = one endpoint path. Cross-domain aggregation belongs at the gateway/agent layer, not in a single sprawling endpoint.

## 7. AgentCore-Specific Naming (AWS Bedrock AgentCore Gateway)

When a server is fronted by AgentCore Gateway, additional rules apply on top of everything above.

1. **Target name is a naming decision, not an afterthought.** Because the exposed tool becomes `${target_name}___${tool_name}`, the target name is permanently visible to every agent and is referenced in Cedar authorization policies (`TargetName___tool_name`). Choose it deliberately and treat it as stable — renaming a target renames every tool and breaks policies and agent code.
2. **Obey the target regex:** `([0-9a-zA-Z][-]?){1,100}` — alphanumeric and hyphens only, **no underscores** in the target segment. Recommended form: domain-first PascalCase or kebab, e.g. `ClaimsCore` or `claims-core`. (Underscores are illegal here even though they're legal in the tool half.)
3. **Do not repeat the domain on both sides.** If the target is `ClaimsCore`, the underlying tool can be `claim_get`, yielding the clean `ClaimsCore___claim_get`. Avoid `ClaimsCore___claims_claim_get`.
4. **Budget total length.** Target (≤100) + `___` + tool (≤64) can produce very long fully-qualified names; keep the *sum* comfortable (aim ≤ ~60–70 total) so downstream clients and logs stay readable.
5. **Gateway name:** lowercase kebab, domain-scoped, remembering AWS appends a 10-char unique suffix — choose ≤48 meaningful chars, e.g. `acme-claims-gw` (becomes `acme-claims-gw-a1b2c3d4e5`).
6. **Strip the prefix in your handler.** Your Lambda/target handler must remove the `target___` prefix before dispatching, per AWS's documented pattern — bake this into the shared handler template so no team forgets it.
7. **Keep target names environment- and account-scoped, not name-scoped where possible.** Prefer separate AWS accounts/gateways per environment over encoding `prod`/`dev` into the target name; if you must encode it, keep it consistent (`ClaimsCoreProd`).

## 8. Prefix & Suffix Cheat-Sheet

1. **Domain prefix (tools):** `claims_`, `member_`, `care_` — first token of every tool name; the primary collision guard.
2. **Reverse-DNS prefix (servers):** `com.acme.<domain>/` — registry namespace and provenance.
3. **Transport prefix (endpoints):** `/mcp/` path segment — identifies MCP traffic uniformly.
4. **Version suffix (endpoints, not tools):** `/v1`, `/v2` in the URL path.
5. **Environment suffix (only when not otherwise isolated):** `-dev`, `-stg`, `-prod`.
6. **Separation-of-duties suffix (servers):** `-read`, `-write`, `-admin`.
7. **Risk suffix (tools, sparingly):** `_admin` / `_danger` only when read/write server split isn't possible.
8. **Avoid entirely:** dots/spaces/slashes/colons in tool names; underscores in AgentCore target names; version numbers baked into tool names by default; technology names (`-springboot`, `-lambda`) in any user-facing name.

## 9. Entities That Should Be Named — Reference Table

| # | Entity | Applies to | Recommended pattern | Example | Hard constraint |
|---|--------|-----------|---------------------|---------|-----------------|
| 1 | **Tool name** | On-prem & AgentCore | `domain_noun_verb`, snake_case | `claims_claim_get` | `^[a-zA-Z0-9_-]{1,64}$`; no dots/spaces |
| 2 | **Tool verb** | Both | Controlled vocab: get/list/search/create/update/delete/submit/check | `..._submit` | — |
| 3 | **MCP server (registry name)** | Both | reverse-DNS: `com.<co>.<domain>/<server>` | `com.acme.claims/claims-core` | Namespace must be owned/verified |
| 4 | **Server short name** | Both | kebab-case, domain-first, capability-named | `claims-core` | Used as client tool prefix |
| 5 | **Read/write split** | Both | `<server>-read` / `<server>-write` | `claims-write` | Cap ~50 tools/server |
| 6 | **Transport endpoint URL** | On-prem (and Gateway host) | `https://<host>/mcp/<server>/v<n>` | `https://mcp.acme.internal/mcp/claims-core/v1` | `/mcp/` + version in path |
| 7 | **MCP host / subdomain** | Both | `mcp.<company>.<env>` | `mcp.acme.prod` | — |
| 8 | **Resource URI** | Both | `<domain>://<type>/<id>` | `claims://claim/12345` | Consistent scheme per domain |
| 9 | **AgentCore Gateway** | AgentCore | lowercase kebab, domain-scoped | `acme-claims-gw` | ≤48 chars (AWS adds `-{10}`) |
| 10 | **AgentCore Target** | AgentCore | domain-first, PascalCase/kebab, **no underscore** | `ClaimsCore` | `([0-9a-zA-Z][-]?){1,100}` |
| 11 | **AgentCore exposed tool** | AgentCore (derived) | `Target___tool` (auto-generated) | `ClaimsCore___claim_get` | triple underscore; used in Cedar policies |
| 12 | **Environment marker** | Both | separate account/registry; else `-prod`/`-dev` suffix | `claims-core-prod` | Be consistent |

## 10. Quick Recommendations

1. **Tools:** `domain_noun_verb`, `snake_case`, ≤ ~40 chars, verb from a controlled list. `claims_claim_get`.
2. **Verbs:** use one closed set where the verb signals the side-effect class — reads (`get`/`list`/`search`/`describe`/`check`/`export`), writes (`create`/`update`/`set`/`upsert`/`submit`), destructive (`delete`/`purge`/`archive`), actions (`send`/`trigger`/`run`/`approve`); one verb per meaning, never mix `get`/`fetch`/`retrieve`.
4. **Servers:** reverse-DNS registry name (`com.acme.claims/claims-core`) with a short kebab capability name; one coherent domain per server; split read/write; cap ~50 tools.
5. **Endpoints:** `https://mcp.<company>.<env>/mcp/<server>/v<n>`; version in the URL path, never in the tool name.
6. **AgentCore targets:** domain-first, **no underscores**, stable forever (they live in tool names and Cedar policies). Don't duplicate the domain across target and tool → `ClaimsCore___claim_get`.
7. **AgentCore gateways:** short lowercase kebab, remember the auto 10-char suffix.
8. **Prefer isolation over encoding:** use separate accounts/registries/subdomains for environments instead of `-prod`/`-dev` suffixes where the platform allows it.
9. **Write it down once, enforce in CI:** publish this as an org standard and lint tool/target names against the two regexes at build time so violations never reach a registry or gateway.

## Sources

| # | Link | What you'll find there | Value |
|---|------|------------------------|-------|
| 1 | [Understand how AgentCore Gateway tools are named — AWS](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway-tool-naming.html) | The authoritative `${target_name}___${tool_name}` rule and the requirement to strip the prefix in handler code | High |
| 2 | [CreateGatewayTarget API — AWS Bedrock AgentCore Control](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateGatewayTarget.html) | Exact target-name regex `([0-9a-zA-Z][-]?){1,100}`, gateway ARN/identifier patterns, description length limits | High |
| 3 | [Tool organization — AWS Prescriptive Guidance (MCP strategies)](https://docs.aws.amazon.com/prescriptive-guidance/latest/mcp-strategies/mcp-tool-strategy-organization.html) | The `domain-noun-verb` naming standard, namespace/collision guidance, the ~50-tool split rule, read/write separation | High |
| 4 | [SEP-986: Specify Format for Tool Names — modelcontextprotocol](https://github.com/modelcontextprotocol/modelcontextprotocol/issues/986) | Spec discussion establishing the tool-name character/length format `^[a-zA-Z0-9_-]{1,64}$` | High |
| 5 | [The MCP Registry — About (modelcontextprotocol.io)](https://modelcontextprotocol.io/registry/about) | Reverse-DNS server-naming convention (`io.github.<user>/<server>`, `com.<company>/<server>`) and namespace ownership | High |
| 6 | [MCP Server Naming Conventions — zazencodes](https://zazencodes.com/blog/mcp-server-naming-conventions) | Empirical data: >90% snake_case, ~95% multi-word tool names, prefixing patterns for multi-tool servers | Medium |
| 7 | [Extending MCP support for Amazon Bedrock AgentCore Gateway — AWS ML Blog](https://aws.amazon.com/blogs/machine-learning/extending-mcp-support-for-amazon-bedrock-agentcore-gateway-2/) | How targets front APIs/Lambda/MCP servers and how tool names surface through the Gateway | Medium |
| 8 | [5 Best Practices for Building MCP Servers — Snyk](https://snyk.io/articles/5-best-practices-for-building-mcp-servers/) | Verb-noun naming, prefixing, and consistency guidance for discoverability | Medium |

## Revision History

| Version | Date | Author | Change |
|---------|------|--------|--------|
| 1.0 | 2026-07-15 | Satya Komatineni | Initial publication |

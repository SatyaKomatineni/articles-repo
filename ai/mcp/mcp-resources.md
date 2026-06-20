<!-- created: 2026-06-20 | updated: 2026-06-20 | topic: AgentCore | keywords: MCP resources, reference links, AgentCore, Entra, OAuth standards -->

# MCP Resources — Curated Reference Links

> A living link library for the Health Payer Agentic Platform research. Each section uses the project reference format. Links are grouped by area; the **Value** column reflects how authoritative the source is (primary specs/official docs = High, vendor/community commentary = Medium/Low). Companion to the [Requirements Specification](./health-payer-agentic-platform-requirements.md) and the [MCP-over-API Guidelines](./mcp-api-guidelines.md).

## Questions This Article Answers

- Where are the authoritative MCP protocol, security, and registry references?
- Which OAuth/identity standards underpin MCP authorization?
- What are the official Microsoft Entra and Microsoft 365 Graph references for delegated access?
- Where is the AWS AgentCore documentation for hosting agents and MCP servers?
- What enterprise/governance MCP guidance exists in industry, and how authoritative is it?

---

## 1. Official MCP — Protocol & Docs

| # | Link | What you'll find there | Value |
|---|------|------------------------|-------|
| 1 | [Model Context Protocol — site & docs](https://modelcontextprotocol.io) | The protocol's home: getting started, concepts, develop, specification | High |
| 2 | [MCP docs index (llms.txt)](https://modelcontextprotocol.io/llms.txt) | Machine-readable index of every doc page — best way to discover what exists | High |
| 3 | [Specification (2025-11-25)](https://modelcontextprotocol.io/specification/2025-11-25/index) | The current normative spec: architecture, lifecycle, transports, server/client primitives | High |
| 4 | [Build an MCP server](https://modelcontextprotocol.io/docs/develop/build-server) | Official walkthrough for building a server (tools, resources, prompts, transport) | High |
| 5 | [Design Principles](https://modelcontextprotocol.io/community/design-principles) | The philosophy behind the protocol; useful for judging "MCP-idiomatic" design | Medium |
| 6 | [Architecture overview](https://modelcontextprotocol.io/docs/learn/architecture) | Core concepts: hosts, clients, servers, and how they interact | High |
| 7 | [SDKs](https://modelcontextprotocol.io/docs/sdk) | Official SDKs for building servers and clients | High |

## 2. MCP — Security & Authorization

| # | Link | What you'll find there | Value |
|---|------|------------------------|-------|
| 1 | [Specification — Authorization (2025-11-25)](https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization) | Normative auth model: OAuth 2.1 resource server, PKCE, RFC 9728 metadata, RFC 8707 indicators | High |
| 2 | [Security Best Practices](https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices) | Attack vectors and prescriptive do/don't guidance — closest thing to official "guidelines" | High |
| 3 | [Understanding Authorization in MCP (tutorial)](https://modelcontextprotocol.io/docs/tutorials/security/authorization) | Step-by-step implementation of OAuth 2.1 auth for an MCP server | High |
| 4 | [Enterprise-Managed Authorization (extension)](https://modelcontextprotocol.io/extensions/auth/enterprise-managed-authorization) | Centralized, IdP-driven access control for MCP — maps onto the Entra assumption | High |
| 5 | [OAuth Client Credentials (extension)](https://modelcontextprotocol.io/extensions/auth/oauth-client-credentials) | Machine-to-machine auth for MCP via the client credentials flow | High |
| 6 | [SEP-990 — Enterprise IdP policy controls in MCP OAuth](https://modelcontextprotocol.io/seps/990-enable-enterprise-idp-policy-controls-during-mcp-o) | Proposal enabling enterprise IdP conditional-access policy during MCP OAuth flows | Medium |

## 3. MCP — Registry & Ecosystem

| # | Link | What you'll find there | Value |
|---|------|------------------------|-------|
| 1 | [Official MCP Registry](https://registry.modelcontextprotocol.io/) | The canonical directory for discovering and distributing MCP servers | High |
| 2 | [Registry — About](https://modelcontextprotocol.io/registry/about) | How the registry works, moderation, and sub-registry model | High |
| 3 | [MCP Blog](https://blog.modelcontextprotocol.io) | Official announcements (registry launch, roadmap, spec changes) | High |
| 4 | [2026 MCP Roadmap](https://blog.modelcontextprotocol.io/posts/2026-mcp-roadmap/) | Direction: transport scalability, governance maturation, enterprise readiness, multi-agent | Medium |

## 4. OAuth & Identity Standards (the backbone)

| # | Link | What you'll find there | Value |
|---|------|------------------------|-------|
| 1 | [RFC 8707 — Resource Indicators for OAuth 2.0](https://datatracker.ietf.org/doc/html/rfc8707) | Audience-binding tokens to a specific resource (one MCP server) | High |
| 2 | [RFC 9728 — OAuth 2.0 Protected Resource Metadata](https://datatracker.ietf.org/doc/html/rfc9728) | How a resource server advertises its authorization server(s) — required by MCP | High |
| 3 | [RFC 8693 — OAuth 2.0 Token Exchange](https://datatracker.ietf.org/doc/html/rfc8693) | Delegation/impersonation token exchange — relevant to cross-service agent calls | High |
| 4 | [OAuth 2.0 Security Best Current Practice (RFC 9700)](https://datatracker.ietf.org/doc/html/rfc9700) | Current security guidance for OAuth deployments (PKCE, token handling) | High |
| 5 | [OWASP API Security Top 10 (2023)](https://owasp.org/API-Security/editions/2023/en/0x11-t10/) | The backend-API threat model the MCP layer must not undermine (BOLA, etc.) | High |

## 5. Microsoft Entra — Identity & OAuth

| # | Link | What you'll find there | Value |
|---|------|------------------------|-------|
| 1 | [Microsoft identity platform docs](https://learn.microsoft.com/en-us/entra/identity-platform/) | Entra ID developer hub: app registration, tokens, flows, MSAL | High |
| 2 | [OAuth 2.0 On-Behalf-Of (OBO) flow](https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-on-behalf-of-flow) | Propagating delegated end-user identity through a chain of APIs | High |
| 3 | [Authentication flows supported in MSAL](https://learn.microsoft.com/en-us/entra/identity-platform/msal-authentication-flows) | Which flows MSAL supports and when to use each | High |
| 4 | [Scopes and permissions](https://learn.microsoft.com/en-us/entra/identity-platform/scopes-oidc) | Delegated vs. application permissions, consent, and scope design | High |
| 5 | [Conditional Access overview](https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview) | Policy controls (device, risk, location) that can gate agent/MCP access | Medium |

## 6. Microsoft 365 / Graph

| # | Link | What you'll find there | Value |
|---|------|------------------------|-------|
| 1 | [Microsoft Graph documentation](https://learn.microsoft.com/en-us/graph/) | The MS365 data/API surface (mail, calendar, files, users, etc.) | High |
| 2 | [Microsoft Graph permissions reference](https://learn.microsoft.com/en-us/graph/permissions-reference) | Every Graph permission, delegated vs. application, for least-privilege scoping | High |
| 3 | [Graph — best practices](https://learn.microsoft.com/en-us/graph/best-practices-concept) | Throttling, paging, consistency, and reliable use of Graph | Medium |

## 7. AWS AgentCore — Agent & MCP Runtime

| # | Link | What you'll find there | Value |
|---|------|------------------------|-------|
| 1 | [Amazon Bedrock AgentCore documentation](https://docs.aws.amazon.com/bedrock-agentcore/) | The managed runtime for deploying/operating agents and MCP servers | High |
| 2 | [Deploy MCP servers in AgentCore Runtime](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-mcp.html) | Creating, testing, and deploying MCP servers on AgentCore Runtime | High |
| 3 | [AgentCore Gateway — MCP server targets](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway-target-MCPservers.html) | Exposing/aggregating MCP servers and APIs through a managed gateway | High |
| 4 | [AgentCore Identity — inbound/outbound auth](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway-target-http-runtime.html) | Two-/three-legged OAuth and IAM SigV4 for agent and MCP connections | High |

## 8. Enterprise MCP — Industry Guidance & Governance

Caveat: these are vendor- and community-authored, not neutral standards. Use for structure and checklists; defer to Sections 1–4 for anything normative.

| # | Link | What you'll find there | Value |
|---|------|------------------------|-------|
| 1 | [MCP Governance Framework at Scale (GitGuardian)](https://blog.gitguardian.com/mcp-governance-framework/) | A governance model: auth standards, scope control, secrets lifecycle, audit | Medium |
| 2 | [Implementing MCP in Enterprise Environments (CData)](https://www.cdata.com/blog/implementing-mcp-enterprise-environments) | Practical enterprise rollout considerations and patterns | Low |
| 3 | [Securing MCP: a defense-first architecture (C. Schneider)](https://christian-schneider.net/blog/securing-mcp-defense-first-architecture/) | Threat-led architectural patterns for hardening MCP deployments | Medium |
| 4 | [MCP security spec update — all about auth (Auth0)](https://auth0.com/blog/mcp-specs-update-all-about-auth/) | Readable walkthrough of the June 2025 MCP auth changes | Medium |

## 9. Related Project Documents

These are **internal documents** within this Claude Cowork project (not external web links) — they live alongside this file in the `agentcore/articles/` folder.

| # | Document | What you'll find there | Value |
|---|----------|------------------------|-------|
| 1 | health-payer-agentic-platform-requirements.md | The parent spec: scope, use cases, hosting models, standards areas | High |
| 2 | mcp-api-guidelines.md | The how-to-build standards derived from these references | High |

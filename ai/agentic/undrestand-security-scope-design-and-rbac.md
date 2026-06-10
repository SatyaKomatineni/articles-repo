# Designing OAuth Scopes and RBAC for Database-Backed APIs

*Topic: AgentCore | Date: 2026-06-07*

*See also: [OAuth Demystified](oauth-demystified.md) | [JWT Demystified](jwt-demystified.md) | [OAuth vs OIDC](oauth-vs-oidc.md)*

---

## Questions This Article Answers

- How should I name and structure scopes for a database-backed API?
- What is the right granularity for scopes — too coarse vs too fine?
- Where do OAuth scopes end and application-level access control begin?
- How do I map organizational roles and groups to scopes in an IdP?
- How do I handle data segmentation — ensuring Alice only sees her region's data?
- What is row-level security and how does it relate to OAuth?
- How do service accounts (agents, pipelines) get scopes vs human users?
- What is the layered access control model, and why do you need all layers?
- What are common scope design mistakes and how do I avoid them?

---

## The Core Problem

You have a database. On top of it you have an API. The API has operations — read a customer, write an order, delete an invoice. Different people in your organization should be able to do different things:

- Customer support can read customer records but not delete them
- Finance can read and write invoices but cannot touch customer PII
- Regional managers can only see data for their region
- A billing agent (machine) can read invoices and post payments
- Admins can do everything

OAuth scopes and RBAC together are how you express and enforce this. But they are different tools at different levels, and conflating them causes one of the most common access control design mistakes in API development.

Let's establish the full picture before diving into design.

---

## The Layered Model: Three Distinct Problems

Access control for a database-backed API has three distinct layers, each answering a different question:

```
┌─────────────────────────────────────────────────────────────────────┐
│  Layer 1: CAN THIS CALLER REACH THIS ENDPOINT?                      │
│  Tool: OAuth Scopes                                                 │
│  Question: Does this token have permission to call /customers/read? │
│  Granularity: Coarse (endpoint or operation type)                   │
│  Enforced by: The API, on every request                             │
├─────────────────────────────────────────────────────────────────────┤
│  Layer 2: WHAT ROLE DOES THIS CALLER HAVE WITHIN THE APP?           │
│  Tool: Application RBAC (roles from JWT claims or a roles table)    │
│  Question: Is this user a "support agent" or a "finance manager"?  │
│  Granularity: Medium (role-based feature/action gating)             │
│  Enforced by: The application, after token validation               │
├─────────────────────────────────────────────────────────────────────┤
│  Layer 3: WHICH ROWS OF DATA CAN THIS CALLER SEE?                   │
│  Tool: Row-Level Security (filters applied per user identity)       │
│  Question: Alice is in the West region — show only West orders.    │
│  Granularity: Fine (per-row, per-record, per-tenant)                │
│  Enforced by: The database query, or an ORM filter, per request     │
└─────────────────────────────────────────────────────────────────────┘
```

**The fundamental rule:** OAuth scopes operate at Layer 1. They cannot substitute for Layers 2 and 3. Trying to encode row-level data segmentation into scopes is the wrong tool for the job — it breaks at scale.

Each layer must exist. Each must be correctly designed. They work together but are implemented separately.

---

## Layer 1: Designing OAuth Scopes

### The Three-Part Naming Pattern

The most practical, maintainable scope naming convention is:

```
{api_name}:{resource}:{action}
```

For a database-backed API called `crm`:

```
crm:customers:read
crm:customers:write
crm:customers:delete
crm:orders:read
crm:orders:write
crm:orders:delete
crm:invoices:read
crm:invoices:write
crm:invoices:delete
crm:admin
```

This structure gives you three benefits: it is self-documenting, it allows wildcard grouping (`crm:customers:*`), and it maps naturally to your database tables and HTTP methods.

### Coarse vs Fine — Finding the Right Level

**Too coarse** — one scope for everything:
```
crm:access
```
Useless. Everyone gets the same access. Provides zero meaningful control.

**Too fine** — one scope per database record type and operation combination:
```
crm:customers_in_west_region:read
crm:customers_in_east_region:read
crm:customers_in_north_region:read
...
```
This explodes in complexity and is the wrong tool. Data segmentation is Layer 3's job. Scopes should not encode data values.

**The right level** — resource + action:
```
crm:customers:read    — can read any customer endpoint
crm:customers:write   — can create/update customers
crm:customers:delete  — can delete customers
```

The scope says "you can call the customers write endpoint." Whether you can write *this specific customer* — that's the application's RBAC and the database's row-level logic.

### Define Your Actions Deliberately

Not all APIs need the same action vocabulary. Think about what operations actually exist in your API and which ones require independent gates.

Common patterns:

**CRUD:**
```
read    — GET requests, read-only
write   — POST and PUT (create and update)
delete  — DELETE
```

**Finer grained when needed:**
```
read        — read non-sensitive fields
read:pii    — read personally identifiable information
export      — bulk export (higher risk than single-record read)
write       — standard updates
write:bulk  — bulk writes (higher risk)
delete      — soft-delete or archive
purge       — hard-delete (highest risk)
admin       — configuration, user management
```

Add granularity only where the business genuinely requires a distinction. A scope that nobody ever has denied is noise.

### Hierarchical Grouping with Compound Scopes

Define a convention where broader scopes imply narrower ones. This is not automatic — your API must implement it — but it keeps policies readable.

```
crm:customers         — implies read + write + delete
crm:customers:read    — read only
crm:customers:write   — read + write
crm:customers:delete  — requires explicit grant
```

A token with `crm:customers:write` can read and write but not delete. A token with `crm:customers` has full access. Your API's scope check function implements this hierarchy:

```python
def has_scope(token_scopes: list[str], required: str) -> bool:
    # Direct match
    if required in token_scopes:
        return True
    # Parent scope implies child
    resource = required.rsplit(":", 1)[0]  # "crm:customers:read" → "crm:customers"
    if resource in token_scopes:
        return True
    # Full API scope implies everything
    api = required.split(":")[0]  # "crm:customers:read" → "crm"
    if f"{api}:admin" in token_scopes:
        return True
    return False
```

### Audience Separation for Multiple APIs

If you have multiple APIs backed by the same database (a CRM API, a reporting API, a billing API), use the `aud` claim to ensure tokens issued for one API cannot be replayed against another.

Each API registers its own identifier (audience) with the IdP:

```
crm-api.mycompany.com
reporting-api.mycompany.com
billing-api.mycompany.com
```

A token with `aud: crm-api.mycompany.com` will be rejected by the reporting API even if it carries the right scopes. This is defense in depth — scopes restrict what you can do, audience restricts which API you can do it against.

---

## Layer 2: Application RBAC — Roles and Groups

Scopes tell the API which endpoints a caller can reach. Application RBAC answers what they can do within those endpoints — which features, which operations, which data categories.

### Where Roles Come From

Roles typically arrive in the JWT as custom claims set by the IdP:

```json
{
  "sub": "alice-id",
  "scope": "crm:customers:read crm:orders:read",
  "role": "support-agent",
  "department": "customer-success",
  "region": "west"
}
```

The IdP (Okta, Azure AD, Cognito) is configured to include these claims based on the user's group memberships. Alice is in the "Support Agents" group in Okta → Okta includes `"role": "support-agent"` in her token automatically.

### The Role Hierarchy

Define roles that map to real job functions, not to technical permissions:

```
support-agent       — reads customer data, logs cases
support-lead        — all support-agent permissions + can escalate
finance-analyst     — reads invoices and payment data
finance-manager     — all analyst permissions + can approve payments
regional-manager    — reads data scoped to their region
data-admin          — bulk operations, exports
system-admin        — full access including user management
```

The application enforces these roles. After token validation and scope check, the application checks the role claim to gate specific features:

```python
@app.route("/customers/<id>/merge", methods=["POST"])
@require_scope("crm:customers:write")  # Layer 1: scope check
@require_role("support-lead")          # Layer 2: role check
def merge_customers(id):
    ...
```

### The Scope + Role Relationship

Scopes and roles are complementary, not redundant. A caller must pass both:

```
Scope check:  "Does this token have crm:customers:write?"  → Is the door unlocked?
Role check:   "Is this caller a support-lead?"             → Are you allowed in this room?
```

A support agent has the `crm:customers:write` scope but not the `support-lead` role — the merge endpoint is inaccessible to them even though their scope would technically permit write operations. The role gate adds a second layer.

This matters especially for service accounts (agents, pipelines): a billing agent might have the `crm:invoices:write` scope but would never have the `support-lead` role. Mixing the two in a single scope would be unwieldy.

---

## Layer 3: Row-Level Security — Data Segmentation

This is where it gets interesting and where most access control designs fail. The first two layers determine *whether* a caller can invoke an operation. Row-level security determines *which records* they see when they do.

### The Problem Scopes Cannot Solve

Consider a West region manager. Their scope is `crm:orders:read`. That's correct — they should be able to read orders. But they should only see West region orders.

You cannot encode this in a scope (`crm:orders:read:west_region`) because:
- Data values (region names, customer IDs, account IDs) do not belong in scopes
- New regions would require new scopes — the IdP becomes a configuration nightmare
- A token is static for its lifetime; data ownership is dynamic

The right model: the scope grants access to the endpoint; the application filters the data based on the caller's identity.

### Pattern 1: Identity-Carried Filters

The JWT carries the caller's data context as claims:

```json
{
  "sub": "alice-id",
  "scope": "crm:orders:read",
  "role": "regional-manager",
  "region": "west",
  "tenant_id": "acme-corp"
}
```

The API extracts these claims and applies them as WHERE clauses:

```python
@app.route("/orders", methods=["GET"])
@require_scope("crm:orders:read")
def list_orders():
    caller = get_token_claims()
    
    query = db.session.query(Order)
    
    # Apply row-level filter from JWT claims
    if caller.role == "regional-manager":
        query = query.filter(Order.region == caller.region)
    elif caller.role == "support-agent":
        query = query.filter(Order.assigned_agent_id == caller.sub)
    # system-admin: no filter, sees everything
    
    return jsonify(query.all())
```

The filter is applied transparently to every query. The caller never knows the filter exists — they just receive data scoped to what they own.

### Pattern 2: Database Row-Level Security (RLS)

Modern databases (PostgreSQL, SQL Server, Oracle) support RLS natively. The database itself applies the filter before returning data to the application. The application sets the current user's identity as a session variable, and the database applies matching policies.

PostgreSQL example:

```sql
-- Define the policy
CREATE POLICY orders_region_policy ON orders
  USING (region = current_setting('app.user_region'));

-- Enable RLS on the table
ALTER TABLE orders ENABLE ROW LEVEL SECURITY;

-- Application sets context before querying
SET app.user_region = 'west';
SELECT * FROM orders;  -- automatically returns only west region orders
```

The advantage: the filter is enforced at the database level, not the application level. Even if an application bug bypasses the application-layer filter, the database enforces it. Defense in depth.

### Pattern 3: Multi-Tenant Segmentation

If you serve multiple organizations from one database (SaaS), every row typically has a `tenant_id`. The rule is absolute: a user from Tenant A must never see Tenant B's data.

This is too important to leave to application code. Implement it at both layers:

**In the JWT:**
```json
{ "tenant_id": "acme-corp-uuid" }
```

**In every query:**
```python
# Applied automatically by ORM middleware — not in individual endpoint code
def apply_tenant_filter(query):
    return query.filter(Model.tenant_id == current_user.tenant_id)
```

**In the database:**
```sql
CREATE POLICY tenant_isolation ON orders
  USING (tenant_id = current_setting('app.tenant_id')::uuid);
```

Triple enforcement: the JWT carries the tenant, the application filters by it, the database enforces it. No single bug creates a data leak.

---

## Administering Scopes: The IdP Side

You have designed your scopes. Now you need to configure your IdP to issue them correctly.

### Registering Scopes with the IdP

Every scope your API uses must be registered with the Authorization Server. In Okta, Azure AD, Cognito, and Auth0, this is done in the API / Resource Server settings:

```
API:   crm-api.mycompany.com
Scopes:
  crm:customers:read      "Read customer records"
  crm:customers:write     "Create and update customers"
  crm:customers:delete    "Delete customers"
  crm:orders:read         "Read order records"
  crm:orders:write        "Create and update orders"
  crm:orders:delete       "Delete orders"
  crm:invoices:read       "Read invoice records"
  crm:invoices:write      "Create and update invoices"
  crm:admin               "Full administrative access"
```

This registration does two things: it tells the IdP which scopes exist (so it can validate requests), and it creates the vocabulary for assigning scopes to clients and users.

### Mapping Groups to Scopes (Human Users)

The standard pattern: groups in the IdP map to scopes that appear in the token.

```
Group: Support Agents
  → Token scopes: crm:customers:read, crm:orders:read
  → Token claims: role=support-agent

Group: Finance Team
  → Token scopes: crm:invoices:read, crm:invoices:write
  → Token claims: role=finance-analyst

Group: Finance Managers
  → Token scopes: crm:invoices:read, crm:invoices:write, crm:invoices:delete
  → Token claims: role=finance-manager

Group: Admins
  → Token scopes: crm:admin
  → Token claims: role=system-admin
```

When Alice in the "Support Agents" group logs in, the IdP sees her group memberships, maps them to scopes, and includes those scopes in her access token. Alice never configures her own scopes — the IdP derives them from her group.

This is the administrative leverage of RBAC: you add Alice to a group and she immediately gets the right scopes. You change the group's scope mapping and every member's token is updated at their next login. Individual user-scope assignments do not scale — groups do.

### Registering Service Accounts (Machine Clients)

AI agents, pipelines, and background services authenticate with Client Credentials (2LO). Each service account is a registered OAuth client with its own client ID, client secret, and assigned scopes.

Unlike users, service accounts do not belong to groups — scopes are assigned directly:

```
Client: billing-agent
  client_id: billing-agent-client-id
  client_secret: [stored in secrets manager]
  Allowed scopes: crm:invoices:read, crm:invoices:write
  Token claims: client_id=billing-agent, role=billing-service

Client: data-export-pipeline
  Allowed scopes: crm:customers:read, crm:orders:read
  Token claims: role=pipeline
```

The service account gets exactly the scopes it needs and nothing more. Principle of least privilege: a billing agent that only reads and writes invoices has no ability to touch customer records, even if it were compromised.

### Scope Consent for User-Facing OAuth

When users authorize a third-party application (Authorization Code flow), the consent screen lists the scopes that application is requesting. The user can approve or deny the request.

For internal enterprise applications, you typically pre-authorize the consent (admins approve scopes on behalf of all users). Individual consent screens are for external developer-facing OAuth flows.

---

## Putting It Together: A Complete Example

Let's trace a request from an AI billing agent to a `GET /invoices` endpoint.

**Setup:**
- Billing agent has client credentials registered in Okta
- Okta is configured to issue `crm:invoices:read` for the billing agent client
- `crm:invoices:read` scope is registered in the CRM API
- API has a Cedar/RBAC rule: `role=billing-service` can read invoices
- Invoices table has RLS: `tenant_id = current_setting('app.tenant_id')`

**Request flow:**

```
1. Billing agent → Okta: POST /token
   { grant_type: client_credentials, client_id, client_secret,
     scope: "crm:invoices:read" }

2. Okta → billing agent: { access_token: JWT }
   JWT payload:
   { iss: "https://mycompany.okta.com",
     aud: "crm-api.mycompany.com",
     sub: "billing-agent-client-id",
     scope: "crm:invoices:read",
     role: "billing-service",
     tenant_id: "acme-corp-uuid",
     exp: [15 minutes from now] }

3. Billing agent → CRM API: GET /invoices
   Authorization: Bearer <JWT>

4. CRM API — Layer 1 (scope check):
   token.scope contains "crm:invoices:read"? ✅

5. CRM API — Layer 2 (role check):
   token.role == "billing-service" → allowed to list invoices ✅

6. CRM API — Layer 3 (row-level filter):
   SET app.tenant_id = token.tenant_id  ("acme-corp-uuid")
   SELECT * FROM invoices
   → Database RLS automatically filters to acme-corp invoices only ✅

7. Response: invoices for acme-corp, no other tenant's data
```

Three independent layers all enforce their part of the contract. A bug in any one layer is caught by the others.

---

## Common Mistakes

**Encoding data values in scopes.** `crm:orders:read:west_region` is wrong. Data values in scopes lead to scope explosion and are the wrong level of abstraction. Use JWT claims + row filters instead.

**One mega-scope.** `crm:full_access` for everyone on the team. Provides zero meaningful control and violates least privilege. If a token is stolen, the attacker has everything.

**Role = scope (one-to-one mapping).** Defining a scope called `support-agent-access` that maps exactly to the support agent role conflates two different concepts. Scopes should represent capabilities on the API; roles should represent job functions. They often map many-to-many.

**No audience check.** Tokens for `crm-api` being accepted by `billing-api` because neither checks `aud`. Prevents token replay attacks.

**Row-level logic only in the application.** Application code can have bugs. A missing WHERE clause returns all rows. Implement RLS at the database layer as the backstop.

**Service accounts with user-level scopes.** A billing pipeline that runs as a "user" in the support-agents group, inheriting all support permissions. Service accounts should be registered as separate OAuth clients with explicitly minimal scopes.

**Scope changes require re-login.** Scopes are baked into the access token at issuance. If you change a user's group (removing a scope), they retain the old scope until their token expires and they get a new one. Design for this: keep access token lifetimes short (15–60 minutes) so scope changes take effect quickly.

---

## A Reference Architecture

```
                        ┌─────────────────────┐
                        │    Identity Provider │
                        │  (Okta / Azure AD)  │
                        │                     │
                        │  Groups → Scopes    │
                        │  Users → Groups     │
                        │  Services → Scopes  │
                        └──────────┬──────────┘
                                   │ issues JWT with
                                   │ scopes + role + tenant_id
                                   ▼
              ┌────────────────────────────────────────┐
              │           CRM API (Resource Server)    │
              │                                        │
              │  Layer 1: Validate JWT signature       │
              │           Check iss, aud, exp          │
              │           Check required scope         │
              │                                        │
              │  Layer 2: Check role claim             │
              │           Gate features by role        │
              │                                        │
              │  Layer 3: Extract tenant_id, region    │
              │           Apply to every DB query      │
              └────────────────────┬───────────────────┘
                                   │ filtered query
                                   ▼
              ┌────────────────────────────────────────┐
              │              Database                  │
              │                                        │
              │  Row-Level Security policies           │
              │  Tenant isolation enforced at DB layer │
              │  No cross-tenant data leakage possible │
              └────────────────────────────────────────┘
```

---

## Summary

| Concern | Tool | Where enforced |
|---|---|---|
| Can this caller reach this endpoint? | OAuth scopes | API, on every request |
| What features/operations does this caller have? | Application RBAC (role claim) | Application, after token check |
| Which rows of data can this caller see? | Row-level security (JWT claims + DB filters) | DB query / database RLS |
| Which users get which scopes? | Group membership in IdP | IdP, at token issuance |
| Which service accounts get which scopes? | Client registration in IdP | IdP, at token issuance |
| How does data segmentation work? | Tenant/region claims in JWT → WHERE clause | Application + database |

---

## Bibliography

| # | Resource | Link | What You'll Find | Quality |
|---|---------|------|-----------------|---------|
| 1 | **OAuth 2.0 Scopes — Auth0 Guide** | [auth0.com/docs/get-started/apis/scopes](https://auth0.com/docs/get-started/apis/scopes) | Practical scope design guidance: naming conventions, custom API scopes, OIDC scopes, and how to configure them in Auth0. The most practically useful starting point. | High |
| 2 | **OAuth 2.0 for APIs — Okta Developer Guide** | [developer.okta.com/docs/guides/implement-oauth-for-okta](https://developer.okta.com/docs/guides/implement-oauth-for-okta/) | How to register API scopes, assign them to groups, and configure Okta to include custom claims in tokens. The administrative setup reference. | High |
| 3 | **Resource Indicators for OAuth 2.0 — RFC 8707** | [rfc-editor.org/rfc/rfc8707](https://www.rfc-editor.org/rfc/rfc8707) | The standard for per-resource-server audience in multi-API deployments. Read when you have multiple APIs sharing one Authorization Server and need proper audience isolation. | High |
| 4 | **PostgreSQL Row-Level Security** | [postgresql.org/docs/current/ddl-rowsecurity.html](https://www.postgresql.org/docs/current/ddl-rowsecurity.html) | Complete reference for PostgreSQL RLS policies. The definitive guide for implementing database-enforced row filtering. | High |
| 5 | **Role-Based Access Control Design Patterns** | [auth0.com/docs/manage-users/access-control/rbac](https://auth0.com/docs/manage-users/access-control/rbac) | Auth0's guide to RBAC: roles, permissions, and mapping users to roles. Practical setup guide for the IdP side of RBAC. | High |
| 6 | **Microsoft Azure AD — App Roles and Groups** | [learn.microsoft.com/en-us/entra/identity-platform/howto-add-app-roles-in-apps](https://learn.microsoft.com/en-us/entra/identity-platform/howto-add-app-roles-in-apps) | How to define app roles in Azure AD, assign them to users and groups, and receive them as claims in tokens. The Azure-specific RBAC administration reference. | High |
| 7 | **OWASP — Broken Object Level Authorization (BOLA)** | [owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization](https://owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization/) | OWASP's #1 API security risk — what happens when row-level authorization is missing or broken. Essential reading for understanding why Layer 3 matters. | High |
| 8 | **OWASP — Broken Function Level Authorization** | [owasp.org/API-Security/editions/2023/en/0xa5-broken-function-level-authorization](https://owasp.org/API-Security/editions/2023/en/0xa5-broken-function-level-authorization/) | OWASP's guide on missing endpoint-level access control — what happens when Layer 1 scope checks are absent or incomplete. | High |
| 9 | **Principle of Least Privilege — NIST** | [csrc.nist.gov/glossary/term/least_privilege](https://csrc.nist.gov/glossary/term/least_privilege) | The foundational security principle behind scope minimization. Brief but authoritative. | High |
| 10 | **Zanzibar: Google's Consistent, Global Authorization System** | [research.google/pubs/pub48190](https://research.google/pubs/pub48190/) | Google's paper on their global RBAC and relationship-based access control system (the basis for systems like OPA and SpiceDB). Relevant when your RBAC requirements outgrow simple role-scope mappings. | High |
| 11 | **Open Policy Agent (OPA)** | [openpolicyagent.org](https://www.openpolicyagent.org/) | A policy engine (similar to Cedar) for fine-grained authorization logic at Layers 2 and 3. Used by large-scale systems where application RBAC needs to be externalized from application code. | High |
| 12 | **Multi-Tenant SaaS Authorization Patterns — AWS** | [aws.amazon.com/blogs/apn/multi-tenant-authorization-with-amazon-cognito](https://aws.amazon.com/blogs/apn/multi-tenant-authorization-with-amazon-cognito/) | AWS guide on tenant isolation patterns using Cognito — how to encode tenant IDs in tokens and enforce them at the API and database layers. | High |

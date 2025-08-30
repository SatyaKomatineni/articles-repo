<!-- ************************************************************ -->
# MS GenAI Licensing Primer
<!-- ************************************************************ -->

Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.

This primer summarizes how Microsoft typically licenses and meters enterprise AI capabilities, and offers quick ranges and references. It keeps the detail from our chat while organizing it for reuse.

**TL;DR**

In an advanced organization, if I were to summarize this, 70% will come from per user license and 30% will come from usage (as of now). So in a 1000 people company, at $30 a month, say $400 a year per user, $120 in usage per user, say $500 per user per year, comes to $500,000 a year. That gives Chat over company data, Agents and tasks for automation using GenAI.

Note: Very simplistic, perhaps, calculations.

- [MS GenAI Licensing Primer](#ms-genai-licensing-primer)
- [Overview](#overview)
- [Two-Layer Model (Per‑User + Usage)](#two-layer-model-peruser--usage)
- [Microsoft 365 Copilot (Per‑User)](#microsoft-365-copilot-peruser)
- [Copilot Studio (Message‑Metered)](#copilot-studio-messagemetered)
- [Azure OpenAI (Token or PTU)](#azure-openai-token-or-ptu)
- [Azure AI Search (Service SKU)](#azure-ai-search-service-sku)
- [Connectors \& Index Quota (Graph/Copilot Connectors)](#connectors--index-quota-graphcopilot-connectors)
- [Typical Spend Mix (Per‑User vs Usage)](#typical-spend-mix-peruser-vs-usage)
- [Example Monthly Bundles](#example-monthly-bundles)
- [Additional Detail](#additional-detail)
- [How that math breaks down](#how-that-math-breaks-down)
- [Usage Vs Per User Percentage breakdown](#usage-vs-per-user-percentage-breakdown)
    - [Typical split (ballpark)](#typical-split-ballpark)
    - [Why it skews this way](#why-it-skews-this-way)
    - [When usage can dominate](#when-usage-can-dominate)
    - [Quick rule of thumb](#quick-rule-of-thumb)
- [Cost Controls \& Gotchas](#cost-controls--gotchas)
- [References](#references)

<!-- ************************************************************ -->
# Overview
<!-- ************************************************************ -->

Microsoft’s enterprise AI stack is usually paid in two ways: a **per‑user add‑on** for Microsoft 365 Copilot and **usage‑based meters** for what you build or run (agents, RAG apps, search services). The per‑user line often dominates total monthly spend once you pass a few hundred users.

<!-- ************************************************************ -->
# Two-Layer Model (Per‑User + Usage)
<!-- ************************************************************ -->

* **Per‑User:** Microsoft 365 Copilot add‑on for eligible M365 plans (unlocks work‑grounded Copilot in Word/Excel/Outlook/Teams).
* **Usage:** Meters tied to what you build/run:

  * **Copilot Studio**: charged by **messages** (tenant packs + optional pay‑as‑you‑go).
  * **Azure OpenAI**: **token** consumption or reserved **Provisioned Throughput Units (PTUs)**.
  * **Azure AI Search** (and other Azure services): billed by **service tier/compute/storage**.
  * **Graph/Copilot connectors**: Microsoft‑built connectors included; **index quota** entitlement is tied to your M365 licenses.

<!-- ************************************************************ -->
# Microsoft 365 Copilot (Per‑User)
<!-- ************************************************************ -->

1. **Price signal**: commonly listed at **\~\$30/user/month (annual)** for Microsoft 365 Copilot. (Regional pricing and promos vary.)
2. **Scope**: enables work‑grounded Copilot in M365 apps and in Teams; web‑only Copilot Chat can be free but doesn’t access tenant content by default.

<!-- ************************************************************ -->
# Copilot Studio (Message‑Metered)
<!-- ************************************************************ -->

1. **Capacity model**: **message packs** (e.g., **25,000 messages for \$200/tenant/month**) plus optional **\$0.01/message pay‑as‑you‑go** once packs are exhausted.
2. **What is a message**: a billable request that triggers an agent response/action (user turn, event, or scheduled run). Different features consume different message counts.
3. **Zero‑rated nuance**: when **M365‑Copilot‑licensed users** use agents **inside M365 apps**, certain operations are “no charge” to Studio capacity; autonomous triggers and non‑M365 channels still meter.

<!-- ************************************************************ -->
# Azure OpenAI (Token or PTU)
<!-- ************************************************************ -->

* **On‑demand (Standard)**: pay by **input/output tokens** per model; optional **Batch API** can discount long‑running jobs.
* **Provisioned (PTU)**: reserve **throughput units** for predictable latency/cost; billed hourly per PTU (monthly/annual reservations available).
* **When to use**: custom RAG apps, API integrations, or heavy automation where you control prompts and throughput.

<!-- ************************************************************ -->
# Azure AI Search (Service SKU)
<!-- ************************************************************ -->

* Billed by **tier** (Free/Basic/Standard/Storage‑optimized) and by **replicas/partitions**. Vector limits and quotas increase with tier.
* Typical enterprise deployments land in **Standard** tiers with one or more replicas; cost scales with indexes, traffic, and vector usage.

<!-- ************************************************************ -->
# Connectors & Index Quota (Graph/Copilot Connectors)
<!-- ************************************************************ -->

* Microsoft‑built connectors are included; your tenant receives an **index quota entitlement** based on your M365 license family.
* For on‑prem sources, the **Graph Connector Agent** securely pushes selected content/metadata to Microsoft 365 for indexing (not federated).
* **Scoping**: use **Restricted SharePoint Search** (allowed‑site list) and **Restricted Content Discovery** to control what Copilot can discover while preserving permissions.

<!-- ************************************************************ -->
# Typical Spend Mix (Per‑User vs Usage)
<!-- ************************************************************ -->

* **Starter (\~200 users, 1–2 agents, light RAG)**: **per‑user 85–90%** / **usage 10–15%**.
* **Core (\~500 users, 3–5 agents, prod RAG)**: **per‑user 75–85%** / **usage 15–25%**.
* **Scaled (\~2,000 users, 10+ agents, multiple RAG apps)**: **per‑user 65–80%** / **usage 20–35%**.
* Usage can dominate (40–60%+) if you run **large custom apps** without buying M365 Copilot or if agent automation is very high‑traffic.

<!-- ************************************************************ -->
# Example Monthly Bundles
<!-- ************************************************************ -->

* **Starter**: \~200 Copilot users + 1–2 agents + small RAG → **\~\$7k–\$9k/mo**.
* **Core**: \~500 users + 3–5 agents + production RAG → **\~\$18k–\$25k/mo**.
* **Scaled**: \~2,000 users + 10+ agents + multiple RAG apps → **\~\$77k–\$107k/mo**.
  *(Ranges reflect \$30/user Copilot, Studio packs, moderate Azure OpenAI/Search usage; actuals vary by region and design.)*

<!-- ************************************************************ -->
# Additional Detail
<!-- ************************************************************ -->

Here’s a quick, realistic **monthly** spend range for a “reasonably AI-enabled” Microsoft shop. I’ll show 3 adoption tiers, then explain the assumptions so you can sanity-check against your org

| Bundle What it looks like Est. monthly |                                                         |                  |
| -------------------------------------- | ------------------------------------------------------- | ---------------- |
| **Starter**                            | 200 Copilot users, 1–2 scoped agents, a small RAG pilot | **\$7k–\$9k**    |
| **Core**                               | 500 Copilot users, 3–5 agents, production RAG           | **\$18k–\$25k**  |
| **Scaled**                             | 2,000 Copilot users, 10+ agents, multiple RAG apps      | **\$77k–\$107k** |

<!-- ************************************************************ -->
# How that math breaks down
<!-- ************************************************************ -->

1. **Microsoft 365 Copilot (per user)** — **\$30/user/month** (annual).
   Examples:
   • 200 users → **\$6,000/mo**
   • 500 users → **\$15,000/mo**
   • 2,000 users → **\$60,000/mo**. [Microsoft](https://www.microsoft.com/en-us/microsoft-365-copilot/pricing?utm_source=chatgpt.com)
2. **Copilot Studio (agents)** — **\$200 per 25k messages/mo** (tenant) + optional **\$0.01/message** PAYG after packs.
   Typical ranges:
   • Starter: **1–2 packs** (25k–50k messages) → **\$200–\$400**
   • Core: **3–5 packs** (75k–125k) → **\$600–\$1,000**
   • Scaled: **10–30 packs** → **\$2,000–\$6,000** (or PAYG). [Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-copilot-studio/billing-licensing?utm_source=chatgpt.com)[Microsoft](https://www.microsoft.com/en-us/microsoft-copilot/blog/copilot-studio/enabling-agents-in-microsoft-365-copilot-chat/?utm_source=chatgpt.com)
3. **Azure OpenAI (custom RAG/apps)** — **consumption by tokens** (or reserved PTUs). A simple way to size:
   **Cost ≈ (input\_tokens ÷ 1M × input\_rate) + (output\_tokens ÷ 1M × output\_rate)**.
   For a mainstream quality model (Azure **GPT-4.1**): **\~\$2/M input tokens** and **\~\$8/M output tokens** (batch is roughly half).
   • Light RAG (e.g., \~10M in / 3M out per month) → **\~\$26/mo** (x2 if not batch)
   • Moderate (e.g., 100M in / 30M out) → **\~\$260/mo** (x2 if not batch)
   • Heavier (e.g., 500M in / 150M out) → **\~\$1.3k/mo** (x2 if not batch)
   Actuals vary widely by prompts and model choice, but this keeps you in the right order of magnitude. [Microsoft Azure+1](https://azure.microsoft.com/pt-pt/pricing/details/cognitive-services/openai-service/?utm_source=chatgpt.com)
4. **Azure AI Search (indexes for RAG)** — billed by **SKU/hour** and scale units. Typical production picks fall from **low-hundreds to low-thousands per month** depending on tier (Basic/S1/S2), replicas and partitions. (Microsoft shows the billing model; exact \$ shows in the portal/region.) [Microsoft Azure](https://azure.microsoft.com/en-us/pricing/details/search/?utm_source=chatgpt.com)[Microsoft Learn](https://learn.microsoft.com/en-us/azure/search/search-sku-manage-costs?utm_source=chatgpt.com)
5. **Copilot/Graph connectors + Graph Connector Agent** — Microsoft-built connectors are included; **index quota** is tied to your M365 entitlements (Microsoft documents the quota policy). Budget impact is usually **\$0** unless you need extra quota or partner connectors. [Microsoft Learn](https://learn.microsoft.com/en-us/microsoftsearch/licensing?utm_source=chatgpt.com)

Reality check: the **per-user Copilot** line nearly always dominates; **agents and RAG infra** are usually **single-digit %** of total—unless you’re running large, high-traffic custom apps.

<!-- ************************************************************ -->
# Usage Vs Per User Percentage breakdown
<!-- ************************************************************ -->

Give and take what is the "typical or likely" breakdown of "per user vs per usage" in terms of their percentages?

<!-- ************************************************************ -->
### Typical split (ballpark)
<!-- ************************************************************ -->

| Adoption tier                                 | Per-user (M365 Copilot) | Usage (agents + Azure OpenAI + AI Search) |
|:----------------------------------------------|:-----------------------:|:-----------------------------------------:|
| **Starter** (≈200 users, 1–2 agents, light RAG) | **85–90%**              | **10–15%**                               |
| **Core** (≈500 users, 3–5 agents, prod RAG)     | **75–85%**              | **15–25%**                               |
| **Scaled** (≈2,000 users, 10+ agents, multiple RAG apps) | **65–80%**      | **20–35%**                               |

<!-- ************************************************************ -->
### Why it skews this way
<!-- ************************************************************ -->

* The **\$30/user/mo** add-on stacks up quickly; a few hundred or thousand users dwarf typical Azure meters.
* **Copilot Studio “message” charges are often muted** when users invoke agents inside M365 (many scenarios are zero-rated), which keeps usage smaller.
* **Azure OpenAI + AI Search** costs grow with traffic/throughput, but for most internal apps they’re still a minority vs per-user.

<!-- ************************************************************ -->
### When usage can dominate
<!-- ************************************************************ -->

* You **don’t buy M365 Copilot** and instead run **custom RAG** apps in Teams/web → **0% per-user, \~100% usage**.
* **High-traffic, automation-heavy** agents (lots of tool calls, background triggers) or **external-facing** apps → usage can climb to **40–60%** of total.

<!-- ************************************************************ -->
### Quick rule of thumb
<!-- ************************************************************ -->

* If you have **≥500 Copilot users**, expect **per-user ≈ 70–90%** of monthly AI spend unless you’re running very heavy custom workloads.

<!-- ************************************************************ -->
# Cost Controls & Gotchas
<!-- ************************************************************ -->

* **Design for low message counts** (aggregate tool calls, reuse context, prefer references when possible).
* **Right‑size models** (mix “mini” for routine tasks; reserve PTUs only where needed).
* **Batch/async where possible** (lower token rates, smoother throughput).
* **Scope discovery early** (Restricted SharePoint Search; clean up oversharing).
* **Track index quota** and prune stale connections/schemas.
* **Instrument** prompts, tokens, message counts; set per‑app budgets and alerts.

<!-- ************************************************************ -->
# References
<!-- ************************************************************ -->

* [Microsoft 365 Copilot pricing](https://www.microsoft.com/en-us/microsoft-365-copilot/pricing) — Official pricing page for Copilot plans.
* [Copilot Studio licensing](https://learn.microsoft.com/en-us/microsoft-copilot-studio/billing-licensing) — Message packs and pay‑as‑you‑go rates.
* [Copilot Studio billing rates & message management](https://learn.microsoft.com/en-us/microsoft-copilot-studio/requirements-messages-management) — What counts as a message and feature‑level rates/zero‑rating.
* [Azure OpenAI pricing](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/openai-service/) — Token pricing, Batch API note, and model availability.
* [Provisioned throughput (PTU) concepts](https://learn.microsoft.com/en-us/azure/ai-foundry/openai/concepts/provisioned-throughput) — How PTUs work and billing.
* [Azure AI Search pricing](https://azure.microsoft.com/en-us/pricing/details/search/) — Tier pricing; see also [Choose a service tier](https://learn.microsoft.com/en-us/azure/search/search-sku-tier) for quota nuances.
* [Copilot/Graph connectors licensing & index quota](https://learn.microsoft.com/en-us/microsoftsearch/licensing) — Index quota entitlements by license family.
* [Restricted SharePoint Search](https://learn.microsoft.com/en-us/sharepoint/restricted-sharepoint-search) — Scope Copilot/search to an allowed list of sites.
* [Allowed list for Restricted SharePoint Search](https://learn.microsoft.com/en-us/sharepoint/restricted-sharepoint-search-allowed-list) — Admin steps and limits.

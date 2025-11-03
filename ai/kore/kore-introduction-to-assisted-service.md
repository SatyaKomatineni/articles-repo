<!-- ********************* -->

# Kore.ai Conversational Architecture in an Assisted Service Scenario

<!-- ********************* -->

**Note:** This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.

This document explains how **Kore.ai** functions in a typical **assisted-service conversation flow** — where a customer begins a chat in self-service mode and is then escalated to a live human agent. It highlights what modules “come alive” within the Kore.ai platform, what is managed by external systems, and how the end-to-end orchestration works across channels.

---

**Contents**

* Overview
* Step-by-Step Conversation Flow
* Active Modules in Kore.ai
* Responsibilities Split: Kore.ai vs External Systems
* Essential Architectural Summary
* Phone Call with Agent + Agent-Chat Assist (Kore.ai) — Step-by-Step
* References

---

<!-- ********************* -->

# Overview

<!-- ********************* -->

In a modern omnichannel contact center, **Kore.ai** acts as the *intelligent conversation fabric* that connects the **self-service layer** (virtual agents) with the **assisted-service layer** (human agents). It orchestrates context, intent, and automation between user interfaces (chat or IVR), human consoles (agent desktops), and enterprise systems (CRM, ERP, ticketing).

---

<!-- ********************* -->

# Step-by-Step Conversation Flow

<!-- ********************* -->

| **Stage**                               | **What Happens**                                              | **Kore.ai Role**                                                                                                 |
| --------------------------------------- | ------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **1. User starts in self-service chat** | Customer interacts via web widget (Kore.ai’s or third-party). | Kore.ai’s NLU engine interprets user intent, retrieves answers, executes dialog tasks, and logs session context. |
| **2. Escalation trigger**               | User requests human help, or sentiment/intent threshold met.  | Kore.ai invokes smart routing logic (skill, language, priority) and compiles context package for transfer.       |
| **3. Routing to correct agent**         | Conversation transferred to human queue.                      | Contact Center AI or connected WFM APIs determine available agent; Kore.ai hands off transcript + variables.     |
| **4. Agent accepts chat**               | Live agent joins via Agent Desktop or CRM interface.          | Kore.ai injects previous conversation history, recognized intents, and metadata for a seamless transition.       |
| **5. Real-time assist activates**       | Agent engages with user.                                      | Agent AI module streams conversation to offer response suggestions, knowledge articles, and next-best actions.   |
| **6. Session wrap-up**                  | Agent completes interaction.                                  | Auto-summarization and transcript logging occur; Kore.ai updates CRM via API; analytics metrics recorded.        |
| **7. Continuous learning**              | Conversation stored for improvement.                          | Quality AI analyzes transcript for compliance, training data, and future tuning.                                 |

---

<!-- ********************* -->

# Active Modules in Kore.ai

<!-- ********************* -->

| **Layer**            | **Active Components**                | **Purpose**                                                            |
| -------------------- | ------------------------------------ | ---------------------------------------------------------------------- |
| **Experience Layer** | Web SDK / Channel Connector          | Facilitates chat UI connection and transport.                          |
| **AI Engine**        | NLU + Dialog Manager + Context Store | Handles understanding, slot filling, and context continuity.           |
| **Routing Layer**    | Contact Center AI                    | Determines agent routing and queue logic.                              |
| **Assisted Layer**   | Agent AI                             | Delivers live assist recommendations, KB surfacing, and summarization. |
| **Knowledge Layer**  | Knowledge AI / RAG                   | Powers question answering for both bot and agent.                      |
| **Analytics Layer**  | Quality AI & Reports                 | Monitors conversation quality, performance, and sentiment.             |

---

<!-- ********************* -->

# Responsibilities Split: Kore.ai vs External Systems

<!-- ********************* -->

| **Function**             | **Kore.ai**                              | **External System (CRM / CCaaS)** |
| ------------------------ | ---------------------------------------- | --------------------------------- |
| Chat interface rendering | ✅ (Kore.ai Web SDK)                      | ⚙️ Possible (Genesys, Salesforce) |
| Conversation state & NLU | ✅                                        | ❌                                 |
| Agent routing rules      | ✅ / ⚙️ (via WFM APIs)                    | Shared                            |
| Agent desktop            | ✅ (Kore.ai Agent Desktop) or ⚙️ external | ✅ if CRM-based                    |
| Case / customer data     | ⚙️ via integration                       | ✅ System of record                |
| QA & sentiment analysis  | ✅ (Quality AI)                           | ⚙️ limited in CRM                 |
| Reporting & dashboards   | ✅ Conversational KPIs                    | ✅ Operational KPIs                |

---

<!-- ********************* -->

# Essential Architectural Summary

<!-- ********************* -->

In an assisted-service interaction, **Kore.ai orchestrates the conversation fabric**, ensuring that context from self-service is preserved, enriched, and transferred to a human agent without loss of information.

### One-line summary:

> **Kore.ai bridges the self-service and assisted-service worlds — understanding intent, managing context, routing intelligently, assisting the human, and synchronizing with enterprise systems for a continuous experience.**

### Simplified Visual Flow:

```
[User Chat Widget]
     ↓
[Kore.ai Conversation Engine]
  ├── NLU / Context
  ├── Dialog Flow
  ├── Routing Rules
  ├── Agent Assist
  └── Analytics
     ↓
[Agent Desktop / CRM System]
```

---

<!-- ********************* -->

<!-- ********************* -->

# Phone Call with Agent + Agent-Chat Assist (Kore.ai) — Step-by-Step

<!-- ********************* -->

This section describes the flow when the **customer is on a phone call** (voice) with a human agent, and the **agent has a Kore.ai chat/assist panel** available inside their desktop (Kore.ai Agent Desktop or embedded in a CRM/CCaaS console). The chat is **not customer-facing**; it is an **agent-side assist and side‑chat to the bot**, used for guidance, knowledge, and automations during the call.

| **Stage**                      | **What Happens on the Call**                   | **Kore.ai Role (Agent-Side Chat/Assist)**                                                                                                                                                |
| ------------------------------ | ---------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Call connects**           | Customer reaches a live agent via IVR routing. | Kore.ai attaches to the session via the voice/CCaaS stream (or transcript feed) and opens the **Agent AI** panel alongside the agent’s desktop.                                          |
| **2. Real-time listening**     | Agent greets and verifies the caller.          | **Live transcription ingest** (from CCaaS) → Kore.ai performs **intent, entity, and sentiment** detection; initializes a case context.                                                   |
| **3. Instant guidance**        | Customer states the issue.                     | Agent panel shows **suggested replies**, **KB snippets**, and **next‑best actions**. The agent can click to paste or launch flows.                                                       |
| **4. Agent ↔ Bot side‑chat**   | Agent needs clarification or a template.       | Agent types privately to the **Kore.ai chat** (e.g., “create RMA,” “eligibility?”). Bot executes **secure automations** (CRM/ERP APIs) and returns structured results to the agent only. |
| **5. Data fetch & actions**    | Agent requires account/order details.          | Kore.ai fetches data via connectors (CRM, OMS, ticketing) and can **pre‑fill forms** or create/update records; the agent confirms before committing.                                     |
| **6. Guardrails & compliance** | Regulated disclosures, PII handling.           | **Playbooks** surface required scripts; **PII masking** and **policy prompts** guide the agent; deviations flagged in real time.                                                         |
| **7. Escalations & transfers** | Supervisor assist or warm transfer.            | Kore.ai packages **live context + transcript** for supervisor/next queue; maintains a **single conversation thread** across hops.                                                        |
| **8. Wrap‑up**                 | Agent ends the call.                           | **Auto‑summary** (pre‑ and in‑call) with key fields; pushes to CRM/ticket; updates **Quality AI** metrics.                                                                               |
| **9. Post‑call follow‑ups**    | Notifications or case tasks.                   | Kore.ai can trigger **proactive outreach** (SMS/chat/email) and schedule follow‑ups; learns from outcomes to refine guidance.                                                            |

**Key Notes**

* The agent’s Kore.ai chat is **agent-only**; the caller does not see it.
* Kore.ai can be embedded in CRMs (Salesforce, Zendesk) or CCaaS consoles (Genesys, Amazon Connect, NICE) and works with their transcription feeds.
* All actions initiated from the agent panel can require **explicit agent confirmation** before committing changes to systems of record.

<!-- ********************* -->

# References

<!-- ********************* -->

* [Kore.ai Contact Center AI Overview](https://kore.ai/platform/contact-center-ai/) — Updated overview of Kore.ai’s contact center automation, self-service, and routing architecture.
* [Kore.ai Agent AI Overview](https://kore.ai/platform/agent-ai/) — Official description of real-time agent assist, summarization, and guidance modules.
* [Kore.ai Documentation Portal](https://docs.kore.ai/) — Main developer documentation with integration guides, SDK references, and architectural diagrams.
* [Kore.ai Web SDK Integration Guide](https://docs.kore.ai/docs/bots/channels/channel-web-sdk/) — Embedding, authentication, and message flow for Kore.ai web chat widgets.
* [Kore.ai XO Platform Architecture Guide](https://docs.kore.ai/docs/platform/overview/platform-architecture/) — Updated diagrams and explanation of major modules, APIs, and orchestration layers.
* [Kore.ai Quality AI](https://kore.ai/platform/quality-ai/) — Analytics, compliance scoring, and post-interaction insights.
* [AWS Marketplace: Kore.ai XO Automation](https://aws.amazon.com/marketplace/pp/prodview-f57yymujnc6ae) — High-level architecture and deployment references for Kore.ai on AWS.

*Link validation date: November 2, 2025.*

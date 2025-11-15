# CX AI Platforms: Genesys, Kore.ai, and GenAI —  Analysis and Guidance Research Notes

Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.

This document thinks through a framework explaining how an enterprise could think about the roles of **Genesys**, **Kore.ai**, and a **Homegrown GenAI CX platform**, with  decision principles, capability allocation, strategic lenses, and contrarian viewpoints. It also considers BPM space and how Kore.ai or GenAI can play a role there. It also includes validated references.

**Table of Contents**

* Overview and Positioning of the Three Platforms
* Core Principles for Decision Making
* Platform-by-Platform Deep Strengths
* The MUST-Use Strength of Kore.ai
* Capability-to-Platform Allocation Guide
* Strategic Lenses: Growth, Innovation, Cost, Efficiency
* Contrarian Strategy Views
* Recommended Enterprise Guardrails
* Kore.ai Positioning Beyond the Contact Center
* Kore.ai Equivalent to Microsoft Copilot Studio
* Visual Comparison: Copilot Studio vs Kore.ai
* Architectural Diagram (Text-Based)
* Expanded Comparison: Kore.ai vs Microsoft Copilot vs ServiceNow Now Assist
* References

# Overview and Positioning of the Three Platforms

Genesys, Kore.ai, and your Homegrown GenAI stack each play structurally different roles in a modern customer experience (CX) ecosystem.

**Genesys** functions as the *contact-center fabric*. It handles routing, interaction lifecycle, SLAs, telephony, WEM, and increasingly native AI tied tightly to CC primitives.

**Kore.ai** functions as a *cross-channel AI agent platform*. It allows enterprises to build reusable conversational agents that operate across multiple channels and multiple CC stacks.

**Homegrown GenAI CX** serves as the *enterprise intelligence and governance layer*, encapsulating proprietary data, reasoning, safety, experimentation, and orchestration beyond what vendors offer.

These roles should not overlap unnecessarily. Each should own capabilities that match its strengths.

# Core Principles for Decision Making

Before choosing where a capability should live, apply these guiding rules:

* If the function depends on **CC primitives** (queues, routing, SLAs): place it in **Genesys**.
* If the function requires **reusable AI agents across channels or vendors**: build it in **Kore.ai**.
* If the function involves **proprietary knowledge, cross-domain workflows, or governance**: centralize it in **Homegrown**.
* Avoid duplicating logic across platforms; pick the natural home for each capability.

# Platform-by-Platform Deep Strengths

This section provides an introduction to the deeper strengths of each platform before examining them in detail.

## Genesys

Genesys excels at contact-center reliability, routing, workforce tools, and native AI aligned with interaction data. Its strengths include:

* Telephony, SIP, IVR, and channel entry
* Skills-based and predictive routing
* WFM/QM, compliance recording, agent desktop
* Native AI for summaries, scoring, insights

## Kore.ai

Kore.ai serves as a cross-platform AI agent factory with strong lifecycle management:

* Build once, deploy everywhere
* Multimodal conversational agents
* Agent Assist across CC & non-CC systems
* Prebuilt connectors and multi-LLM support
* Multi-bot orchestration

## Homegrown GenAI

Your homegrown stack houses proprietary intelligence:

* Domain knowledge
* Central RAG pipelines
* Cross-enterprise agentic workflows
* Governance, observability, policies
* Rapid experimentation

Each platform plays a non-redundant structural role.

# The MUST-Use Strength of Kore.ai

Kore.ai’s non-negotiable, unique strength is:

> **It is the only platform of the three that enables you to build AI agents once and deploy them across any channel and any contact center stack.**

This prevents fragmentation of AI logic, reduces duplication, and gives the enterprise long-term portability across vendors and use cases.

This portability becomes essential as CX expands beyond the contact center into HR, IT, web, mobile, claims, operations, and more.

# Capability-to-Platform Allocation Guide

Below is a mapping for enterprise decision-making.

## Genesys Owns

* Telephony, SIP, IVR
* Routing & queues
* Interaction lifecycle
* WEM, QM, recording
* Operational CC analytics
* CC-native AI summarization & scoring

## Kore.ai Owns

* Customer-facing virtual agents
* Enterprise-wide conversational agents
* Multi-channel deployment (web, app, voice, SMS, WhatsApp, Teams, Slack)
* Agent Assist across CC and internal tools
* Bot lifecycle (versioning, testing, rollback)
* Multi-bot orchestration

## Homegrown Owns

* Proprietary knowledge bases
* Enterprise RAG
* Cross-system agentic workflows
* Governance: safety, red-teaming, evaluation
* Experimentation with new models
* Observability across all AI usage

This separation prevents redundancy, reduces cost, and ensures long-term flexibility.

# Strategic Lenses: Growth, Innovation, Cost, Efficiency

## Growth & Innovation

* Keep innovation portable by building AI logic in Kore.ai or homegrown.
* Use Genesys for CC primitives—don’t fight the platform.
* Use homegrown as the R&D engine and domain intelligence center.

## Cost & Efficiency

* Avoid duplicate AI features across platforms.
* Keep high-volume operations vendor-native.
* Use Kore.ai for amortizing conversational investments across all channels.

# Contrarian Strategy Views

## 1. “Genesys is only dialtone and routing.”

This extreme stance forces portability of AI logic and prevents vendor lock-in.

## 2. “Kore.ai is the universal AI layer.”

Kore.ai becomes the default agent platform for customers, employees, and partners.

## 3. “Homegrown is the lock-in layer—intentionally.”

All important business rules and reasoning live in homegrown APIs.

## 4. “Under-use vendor AI features for discipline.”

Sticking to homegrown governance ensures consistency across channels.

# Recommended Enterprise Guardrails

A simple set of rules you can embed in governance:

1. **Genesys owns** routing, queues, WEM, compliance, and CC analytics.
2. **Kore.ai owns** AI agents, agent assist, and all multi-channel conversational UX.
3. **Homegrown owns** proprietary intelligence, RAG, safety, cross-domain workflows.
4. Each new capability must document placement rationale and portability impact.

# Kore.ai Positioning Beyond the Contact Center

Here’s the comprehensive material requested, fully integrated.

Kore.ai is positioning itself beyond the contact center as an **enterprise-wide AI orchestration platform**, competing in adjacencies traditionally held by Microsoft Copilot, OpenAI ChatGPT Enterprise, Google Gemini, ServiceNow, and other workflow automation tools. Its strategy is to function as a **conversational and workflow automation layer** for all enterprise domains, not just customer experience.

## Kore.ai’s Non-CC Strategy

Kore.ai aims to become the **AI Work Orchestrator** for the entire enterprise. This includes HR, IT, Operations, Finance, Employee Support, and more. Their offerings such as AI for Work, Process Apps, Smart Skills, and their Generative AI hub enable:

* Internal knowledge assistants
* Multi-step workflow automation
* Enterprise search and reasoning via LLMs
* Employee self-service agents
* IT/HR/Ops ticket triage and handling

Kore.ai’s ambition is to sit above the application layer, serving as a unified AI engine across internal systems.

## Kore.ai vs Microsoft Copilot

Microsoft Copilot dominates workspace applications and Office 365 data. Kore.ai does **not** compete here directly. Instead, it positions itself as:

> **“The AI layer for every workflow that lives outside Office.”**

Copilot is weak at cross-system workflow orchestration, multi-channel conversational deployments, and enterprise-specific logic. Kore.ai excels in these spaces.

## Kore.ai vs ChatGPT / OpenAI Enterprise

OpenAI provides powerful reasoning, creativity, and search capabilities, but lacks:

* Multi-channel bot building
* Workflow orchestration
* Enterprise governance frameworks
* Connectors and lifecycle management

Kore.ai serves as the **governed enterprise wrapper** around LLMs, enabling safe deployment, flow building, and integration while delegating intelligence to the LLM of choice.

## Kore.ai vs ServiceNow Now Assist

ServiceNow excels in structured workflow automation but lacks conversational depth and multi-channel reach. Kore.ai provides virtual agents and LLM-based logic that sit on top of ITSM/HRSD systems to give them “voice and intelligence.”

## Strengths Beyond the Contact Center

* **Multi-channel reach**: Teams, Slack, SMS, WhatsApp, email, web, mobile
* **Enterprise connectors** for HR, IT, CRM, ERP, identity, etc.
* **LLM orchestration**: Model selection, guarding, grounding, safety
* **Governance**: Policies, audits, data boundaries, observability
* **Rapid deployment**: Low-code builder with versioning and testing

## Limitations and Gaps

Kore.ai does **not** aim to compete at the OS or workplace-suite level. It will not replace:

* Copilot’s deep Office automation
* OpenAI’s superior reasoning engines
* Native desktop productivity assistants

Kore.ai instead thrives as the **enterprise conversational automation hub**, integrating and orchestrating systems rather than replacing workplace productivity tools.

## Position Summary

Kore.ai is best understood as:

> **An enterprise workflow and conversational AI layer that complements Copilot and OpenAI—but does not compete directly with their strengths.**

Kore.ai also positions itself as an enterprise-wide AI automation platform that competes outside the contact center, especially in areas traditionally dominated by Copilot, ChatGPT Enterprise, and ServiceNow. It focuses on enabling AI-driven workflows, internal assistants, and cross-system automation.

## Kore.ai’s Non-CC Strategy

Kore.ai aims to be an enterprise AI work orchestrator by offering tools for HR, IT, Operations, and other internal domains. It provides conversational and workflow automation capabilities that extend well beyond CC interactions.

## Competitors in This Space

While Microsoft Copilot is strong in workplace apps and OpenAI provides powerful reasoning models, Kore.ai differentiates through multi-channel deployment and workflow capabilities. It complements rather than replaces these platforms.

## Strengths Outside Contact Center

* Multi-channel support across Teams, Slack, email, and more
* Workflow automation with LLM integration
* Centralized governance and observability
* Enterprise connectors across major systems

## Limitations and Gaps

Kore.ai does not replace productivity assistants like Copilot or knowledge reasoning tools like ChatGPT. Instead, it serves as a flexible conversational automation layer.

# Kore.ai Equivalent to Microsoft Copilot Studio

Kore.ai offers a set of capabilities that parallel Microsoft Copilot Studio’s ability to design, test, deploy, and govern workflow-driven AI agents. While Microsoft presents these features within a single branded studio, Kore.ai distributes them across several integrated modules. Together, they provide comparable or deeper functionality for enterprise-grade AI automation.

## Kore.ai XO Platform – Bot Builder

This serves as Kore.ai’s primary environment for building conversational and workflow-driven agents.

* Conversational flow design
* Dialog tasks, intents, and entity modeling
* LLM integration and hybrid NLU
* Tool invocation and business logic
* Multichannel publishing (web, WhatsApp, Teams, SMS, etc.)
* A/B testing and analytics

## Kore.ai Process App Builder

A visual workflow automation layer analogous to Copilot Studio’s workflow design.

* Multi-step process flows
* Human-in-the-loop actions and escalations
* Automated actions across systems (ServiceNow, SAP, Oracle, Workday)
* Approval flows and ticket handling
* Enterprise-wide operations automation

## Kore.ai Knowledge AI + RAG Studio

Parallel to Copilot’s grounding and knowledge configuration.

* Ingestion of documents and enterprise knowledge
* Semantic search using LLM embeddings
* FAQ extraction and knowledge normalization
* Source-aware grounding and ranking
* Model-specific knowledge routing

## Generative AI Hub

Acts as Kore.ai’s AI governance framework.

* Model selection and routing (OpenAI, Azure, Claude, Gemini, etc.)
* Prompt policies and templates
* Safety layers and guardrails
* Token controls and usage policies
* Audit and observability tools

## Testing and Lifecycle Management

Kore.ai supports enterprise-grade agent lifecycle controls.

* Automated regression testing
* Scenario simulation and validation
* Versioning and rollback
* Promotion pipelines (Dev → Stage → Prod)
* Cross-bot testing and dependency management

## SmartAssist (Optional)

A packaged solution similar to prebuilt Copilot templates.

* Internal employee assistants
* IT/HR support agents
* Preconfigured workflows and skills
* Quick-deploy domain assistants

## Summary Comparison

Kore.ai’s build–test–deploy–govern ecosystem mirrors Copilot Studio in capability while extending into deeper multichannel deployment and complex workflow orchestration.

# Visual Comparison: Copilot Studio vs Kore.ai

| Capability Area              | Microsoft Copilot Studio                | Kore.ai Equivalent                          |
| ---------------------------- | --------------------------------------- | ------------------------------------------- |
| Conversational Agent Builder | Topics & Dialog Designer                | XO Platform Bot Builder                     |
| Workflow Automation          | AI Actions & Power Automate Integration | Process App Builder                         |
| Knowledge Grounding          | SharePoint/Graph Connectors             | Knowledge AI + RAG Studio                   |
| Model Governance             | Safety Filters, Policies                | Generative AI Hub                           |
| Testing & Lifecycle          | Test Canvas, Versioning                 | Automated Testing & Bot Lifecycle Mgmt      |
| Multichannel Deployment      | Teams, Web, Apps                        | Teams, Slack, Web, WhatsApp, SMS, IVR, Apps |
| Internal Assistants          | Copilot Templates                       | SmartAssist                                 |
| LLM Flexibility              | Azure OpenAI                            | OpenAI, Azure, Anthropic, Gemini, Cohere    |

# Architectural Diagram (Text-Based)

```
          +------------------------------+         +-----------------------------+
          |    Microsoft Copilot Studio  |         |        Kore.ai Platform     |
          +------------------------------+         +-----------------------------+
          |  • Office 365 Integrations   |         |  • Multichannel Surface     |
          |  • Teams as primary channel  |         |  • XO Bot Builder           |
          |  • Power Automate workflows  |         |  • Process Apps             |
          |  • Graph Data grounding      |         |  • Knowledge AI + RAG       |
          |  • Governance via M365 stack |         |  • Generative AI Hub        |
          +------------------------------+         +-----------------------------+
                         |                                          |
                         |  Complementary Roles                     |
                         v                                          v
          +--------------------------------------------------------------------+
          |    Enterprise Workflow & Multi-Channel Automation (Kore.ai)        |
          +--------------------------------------------------------------------+
```

# Expanded Comparison: Kore.ai vs Microsoft Copilot vs ServiceNow Now Assist

## 1. Workplace Productivity Layer

* **Microsoft Copilot** dominates Office, Windows, Teams.
* **Kore.ai** does not compete here.
* **ServiceNow Now Assist** provides inline assistance within the NOW platform.

## 2. General Reasoning & Knowledge

* **OpenAI/Copilot** strongest reasoning models.
* **Kore.ai** delegates reasoning to external LLMs.
* **ServiceNow** focuses on structured enterprise data.

## 3. Workflow Automation

* **Copilot**: limited cross-system workflows; strongest within Microsoft ecosystem.
* **Kore.ai**: strongest cross-enterprise orchestrator; multi-system + multi-channel.
* **ServiceNow**: strongest structured workflows; weak conversational depth.

## 4. Channels and Deployment Surfaces

* **Copilot**: Teams, Office, Windows.
* **Kore.ai**: Teams, Slack, WhatsApp, SMS, Web, IVR, Mobile.
* **ServiceNow**: NOW platform UI.

## 5. Conversational AI

* **Kore.ai** strongest: dialog design, NLU, LLM orchestration.
* **Copilot**: strong LLM responses, weak workflow conversation.
* **ServiceNow**: functional but limited.

## 6. Governance & Control

* **Copilot**: governed via Microsoft 365.
* **Kore.ai**: enterprise-wide governance via AI Hub.
* **ServiceNow**: governance inside NOW platform.

## Summary Position

* **Copilot** = the Workplace Assistant.
* **Kore.ai** = the Enterprise Workflow & Conversational Hub.
* **ServiceNow** = the Structured Process Automation Backbone.

# Kore.ai Agentic Workflows as BPM (Non-Conversational Use Cases)

Kore.ai’s agentic workflow capabilities extend well beyond conversational interactions. Even without any chat or user dialogue, Kore.ai can operate as a powerful, event-driven, LLM-enhanced workflow automation engine—functioning similarly to a next-generation BPM (Business Process Management) platform.

## Event-Driven Workflows (No Conversation Required)

Kore.ai Process Apps can be triggered by events in external systems:

* ServiceNow ticket creation
* Salesforce case update
* Workday record changes
* File uploads or document arrival
* Database changes
* API webhooks
* Scheduled or timed triggers

Once triggered, Kore.ai executes multi-step automation flows that can:

* Call APIs across enterprise systems
* Query knowledge or perform LLM reasoning
* Transform or classify data
* Generate summaries or structured outputs
* Route to humans for approvals
* Update systems of record or send notifications

This mode uses **zero conversational context**.

## Kore.ai as a Cognitive Workflow Engine

Traditional BPM engines use static rules. Kore.ai enhances this by adding LLM-based reasoning that can:

* Choose next workflow steps dynamically
* Identify exceptions or anomalies
* Classify documents or cases
* Extract structured data from unstructured content
* Interpret ambiguous business rules

This enables automation of processes that normally require human judgment.

## System-to-System Integration Workflows

Kore.ai also supports fully headless integrations:

1. Receive a webhook event
2. Run an LLM to understand or classify the input
3. Query enterprise knowledge for rules or policies
4. Execute integrations (Workday, SAP, ServiceNow, custom APIs)
5. Generate content or route operational tasks
6. Log or publish results to downstream systems

These workflows behave like a modern integration layer powered by AI.

## How Kore.ai Mirrors and Extends BPM

| BPM Feature         | Kore.ai Implementation                |
| ------------------- | ------------------------------------- |
| Process flows       | Process App visual builder            |
| Rules engine        | LLM-based decision nodes + conditions |
| Human-in-loop       | Approval and escalation steps         |
| Integrations        | 100+ connectors + API builder         |
| Document processing | OCR + LLM extraction + RAG            |
| Decision gateways   | LLM reasoning as decision engine      |
| Monitoring          | Analytics + workflow traces           |
| Queues              | Built-in task handling                |

Kore.ai thus provides BPM-like automation enhanced with reasoning and multi-system orchestration.

## Example Non-Conversational Use Cases

### HR

* Onboarding automation
* PTO request processing
* Offer letter generation

### IT

* Automated ticket triage
* Incident classification
* Root-cause LLM suggestions

### Finance

* Invoice matching
* PO validation
* Exception routing

### Healthcare / Claims

* Document extraction from faxes
* Claims adjudication workflows
* Appeals summarization and routing

### Compliance

* Document classification
* Audit workflow support
* Report generation

## Why Enterprises Use Kore.ai as BPM

* **LLM-based branching and decisions** allow handling unstructured processes.
* **Multi-agent orchestration** enables parallel, specialized tasks.
* **Rapid build cycles** compared to traditional BPM tools.
* **Unified governance**, observability, and safety controls.
* Optionally expose workflows conversationally on demand.

## Summary

Kore.ai serves as a headless, event-driven, AI-enhanced workflow engine capable of running complex enterprise processes with or without conversational triggers. It effectively acts as a modern BPM platform augmented with LLM reasoning and agentic automation.

# Distinction Between Deterministic BPM and Dynamic GenAI Workflows

Modern enterprises increasingly face a gap between **deterministic, rules-based BPM** and **dynamic, LLM-driven agentic workflows**. This section explains the distinction and how GenAI—whether in Kore.ai, Copilot, or homegrown stacks—changes enterprise workflow design.

## 1. What Is the Distinction?

### Deterministic BPM Workflows

Traditional BPM engines (ServiceNow Flow Designer, Pega, Appian, Camunda):

* Follow strict, predefined paths
* Require explicit decision rules (IF/THEN)
* Assume stable business processes
* Fail or escalate when ambiguity occurs
* Are optimized for structured, repeatable operations

### Dynamic GenAI Workflows

GenAI-driven workflows (Kore.ai Process Apps with LLMs, Copilot orchestration, enterprise agentic runtimes):

* Adapt their next steps based on LLM reasoning
* Handle unstructured inputs and ambiguous situations
* Create micro-flows at runtime (“decide what to do next”)
* Can modify actions based on context, patterns, or learned signals
* Support processes with high variability or exceptions

**In short:**

* **BPM = rules-first automation**
* **GenAI = reasoning-first automation**

Where BPM is rigid but safe, GenAI is flexible but requires governance.

## 2. How Should GenAI Agents Affect Enterprise Use Cases?

GenAI introduces new workflow patterns that BPM could never handle:

* **Dynamic next-step selection** using LLMs
* **Context-sensitive task branching** (e.g., classify, summarize, extract, escalate)
* **Tool invocation based on detected intent** rather than predefined rules
* **Cross-domain reasoning** across HR, IT, Ops, and Compliance
* **On-demand microworkflow creation** (“figure out what this request actually is”)
* **Adaptive exception handling** using knowledge + reasoning

This means business processes no longer need to be fully pre-designed. Instead, GenAI fills the gap between:

* Strict workflow automation (BPM)
* Human judgment (knowledge workers)

GenAI becomes the **bridge layer**.

## 3. Use Cases for Dynamic GenAI Workflows

Below are examples where deterministic BPM breaks down but GenAI thrives.

### HR & People Operations

* Understanding vague employee requests (“I need help with my benefits issue”)
* Extracting data from unstructured onboarding documents
* Determining next action based on policy interpretation

### IT Service Management

* Multi-source incident triage
* Ticket de-duplication based on semantic similarity
* LLM-based root-cause hypothesis generation

### Finance

* Classify non-standard invoices or expense reports
* Interpret non-structured procurement requests
* Generate structured objects from messy inputs

### Healthcare / Claims / Appeals

* Understand medical narratives
* Summarize clinical notes
* Decide routing steps depending on symptoms, exceptions, or appeals rationale

### Compliance & Legal

* Interpret open-ended regulatory queries
* Classify risk scenarios without rigid rules
* Generate workflows dynamically based on document context

In all these, static BPM models would require thousands of rules. GenAI handles them with reasoning.

## 4. Implications and Conclusions

### Key Implications

* **Hybrid BPM + GenAI architectures** will dominate: BPM for structure, GenAI for ambiguity.
* **Agentic workflows reduce the need for explicit modeling**, shifting work from process designers to AI governance teams.
* **Enterprises must build guardrails**, because dynamic workflows can “decide” incorrectly without constraints.
* **Kore.ai becomes a strategic middle layer**, turning unstructured requests into structured steps BPM can execute.
* **Copilot, Kore.ai, and homegrown agents** will increasingly orchestrate tools dynamically, making workflow automation more conversational, contextual, and reasoning-driven.

### Conclusion

Deterministic BPM is essential where stability, repeatability, and compliance matter. GenAI workflows are essential where variability, ambiguity, and human-like judgment are required. The future of enterprise automation is **hybrid**:

> **BPM provides the backbone. GenAI provides the brain.**

# References

**Kore.ai Process Apps (Workflow Automation)**
Describes Kore.ai’s workflow automation capabilities, event triggers, and BPM-like functions.
[https://kore.ai/process-apps/](https://kore.ai/process-apps/)

**Kore.ai Knowledge AI**
Details Knowledge AI, document ingestion, semantic search, and RAG functionality.
[https://kore.ai/knowledge-ai/](https://kore.ai/knowledge-ai/)

**Kore.ai Generative AI Hub**
Explains model governance, safety layers, prompt controls, and LLM routing.
[https://kore.ai/generative-ai-hub/](https://kore.ai/generative-ai-hub/)

**Kore.ai XO Platform Docs (Developer Documentation)**
Covers lifecycle management, versioning, testing, and agent governance.
[https://developer.kore.ai/](https://developer.kore.ai/)

**Microsoft Copilot Studio Overview**
Covers Copilot Studio’s design, testing, workflows, and AI orchestration features.
[https://learn.microsoft.com/microsoft-copilot-studio/](https://learn.microsoft.com/microsoft-copilot-studio/)

**Microsoft Power Automate (Workflow Automation)**
Describes workflow building tools comparable to Kore.ai Process Apps.
[https://learn.microsoft.com/power-automate/](https://learn.microsoft.com/power-automate/)

**ServiceNow Now Assist Documentation**
Provides details on generative AI use cases within structured enterprise workflows.
[https://docs.servicenow.com/bundle/now-assist/page/product/now-assist.html](https://docs.servicenow.com/bundle/now-assist/page/product/now-assist.html)

**ServiceNow Flow Designer (BPM/Workflow)**
Explains structured workflow automation capabilities comparable to Kore.ai’s process flows.
[https://docs.servicenow.com/bundle/tokyo-servicenow-platform/page/administer/flow-designer/concept/flow-designer.html](https://docs.servicenow.com/bundle/tokyo-servicenow-platform/page/administer/flow-designer/concept/flow-designer.html)

**Kore.ai Integrations Marketplace**
Comprehensive directory of enterprise connectors for system-to-system workflow automation.
[https://kore.ai/marketplace/](https://kore.ai/marketplace/)

**Kore.ai Smart Skills & Templates**
Prebuilt workflow logic and assistants used similarly to Copilot templates.
[https://kore.ai/smart-skills/](https://kore.ai/smart-skills/)

**Kore.ai Bots Platform Architecture Overview**
Technical deep dive into bot lifecycle, multi-bot orchestration, and governance.
[https://kore.ai/platform/architecture/](https://kore.ai/platform/architecture/)

**Genesys AI, Cloud, and Experience Orchestration**
Source covers Genesys’ AI-native orchestration, summaries, analytics, and routing capabilities.
[https://www.genesys.com/experience-orchestration/genesis-of-ai](https://www.genesys.com/experience-orchestration/genesis-of-ai)

**Genesys AI Assist and Analytics**
Provides details on native Agent Assist, evaluations, and scoring features.
[https://www.genesys.com/genesys-ai](https://www.genesys.com/genesys-ai)

**Kore.ai XO Platform Overview**
Explains multi-channel AI agents, connectors, lifecycle, and enterprise deployments.
[https://kore.ai/platform/xo-platform/](https://kore.ai/platform/xo-platform/)

**Kore.ai SmartAssist Contact Center**
Describes Kore’s CCaaS offering and how it complements or replaces traditional CC stacks.
[https://kore.ai/smartassist/](https://kore.ai/smartassist/)

**Kore.ai Integrations with Genesys**
Shows how Kore bots integrate with Genesys voice, chat, and agent desktop.
[https://kore.ai/integrations/genesys/](https://kore.ai/integrations/genesys/)

**Kore.ai AI for Work**
Covers enterprise-wide AI workflows beyond CC: HR, IT, operations.
[https://kore.ai/ai-for-work/](https://kore.ai/ai-for-work/)

Each link has been validated as accessible and up-to-date.

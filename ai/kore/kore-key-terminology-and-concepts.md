<!-- ********************* -->

# Kore.ai Terminology and Key Concepts

<!-- ********************* -->

**Note:** This glossary was generated with the assistance of ChatGPT and summarizes common and Kore.ai-specific terms found across official documentation. Please verify definitions against the latest [Kore.ai Docs](https://docs.kore.ai/).

This document provides a structured list of the **most important terminology** and **platform modules** used by Kore.ai in its conversational AI and contact center ecosystem. The terms are grouped into three tiers — Core Concepts, Platform Modules, and Supporting Components.

---

<!-- ********************* -->

# Core Conversational AI Concepts

<!-- ********************* -->

| **Term**                                 | **Definition / Description**                                                                                                | **Why It Matters**                                                   |
| ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| **Intent**                               | Represents the goal or purpose of a user’s message (e.g., *Check Balance*).                                                 | Central to understanding user objectives and routing conversations.  |
| **Entity**                               | Specific data fields required to complete an intent (e.g., *Date*, *Location*).                                             | Enables bots to extract actionable details from user inputs.         |
| **NLU (Natural Language Understanding)** | The AI process that identifies intent and extracts entities from free-form text or speech.                                  | Powers the bot’s ability to understand varied user utterances.       |
| **Dialog Task / Experience Flow**        | A defined conversational workflow for handling an intent, including questions, logic, and responses.                        | Core building block for structured bot interactions.                 |
| **Knowledge Graph / Knowledge AI**       | Organized knowledge base used for FAQ-style or informational queries via retrieval or RAG (Retrieval-Augmented Generation). | Provides intelligent, factual responses with contextual relevance.   |
| **Memory / Session Context**             | Stored information that keeps track of a user’s conversation history and variables during sessions.                         | Enables multi-turn, contextual conversations.                        |
| **Omnichannel Experience**               | Seamless user interaction across multiple channels (web chat, IVR, SMS, WhatsApp, Teams, etc.) with shared context.         | Ensures continuity and consistent experience regardless of platform. |
| **Sentiment Analysis**                   | AI analysis of user tone or emotion during a conversation.                                                                  | Guides escalation, tone adjustment, and agent-assist triggers.       |
| **Proactive vs Reactive References**     | Reactive = user-initiated; Proactive = system-initiated engagement (alerts, reminders).                                     | Enables outbound messaging and re-engagement use cases.              |
| **ASR (Automatic Speech Recognition)**   | Converts user speech into text for NLU processing in voice interactions.                                                    | Vital for IVR and voice bot experiences.                             |

---

<!-- ********************* -->

# Kore.ai Platform Modules and Offerings

<!-- ********************* -->

| **Module / Product**        | **Purpose**                                                                                                        | **Key Capabilities**                                                                      |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------- |
| **XO Platform**             | Kore.ai’s unified development and runtime platform for building, training, and deploying conversational AI agents. | Low-code builder, NLU training, Dialog Tasks, Knowledge AI, integration management.       |
| **Agent AI (Agent Assist)** | Provides real-time recommendations, knowledge suggestions, and auto-summarization for live human agents.           | Intent/sentiment monitoring, contextual help, proactive guidance, and compliance prompts. |
| **Agent Desktop**           | Unified workspace for agents to handle voice, chat, or social interactions.                                        | Displays full conversation history, knowledge snippets, and automation triggers.          |
| **Contact Center AI**       | Module focused on routing, queue management, and assisted-service transitions.                                     | Skill-based routing, escalation logic, and queue optimization.                            |
| **Quality AI**              | Evaluates 100% of conversations (voice + chat) for compliance, tone, and performance metrics.                      | Sentiment scoring, topic tracking, supervisor dashboards.                                 |
| **Knowledge AI**            | Manages and retrieves knowledge articles across enterprise systems using generative retrieval.                     | Contextual answers, summarization, RAG pipeline integration.                              |
| **Voice Gateway**           | Kore.ai’s telephony and IVR integration service for handling voice calls with ASR + TTS.                           | Dynamic IVR, speech recognition, telephony routing.                                       |
| **Proactive Engagement**    | Enables outbound or triggered communications through voice, SMS, or chat.                                          | Event-based triggers, notifications, and follow-up flows.                                 |
| **Analytics & Insights**    | Reporting and visualization tools within XO Platform.                                                              | Conversation KPIs (FCR, CSAT, AHT), containment rate, escalation tracking.                |

---

<!-- ********************* -->

# Difference Between Agent Assist and Agent Desktop

<!-- ********************* -->

**Agent Assist (Agent AI)** and **Agent Desktop** are complementary modules within Kore.ai’s contact center stack. The **Agent Assist** component provides real-time intelligence and automation, while the **Agent Desktop** serves as the operational workspace for agents handling customer interactions.

| **Aspect**           | **Agent Assist (Agent AI)**                                                                                                                        | **Agent Desktop**                                                                                                                                |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Primary Role**     | Cognitive AI engine offering contextual support, guidance, and automation to the agent during live conversations.                                  | Unified console where agents manage chats, calls, and social messages with full visibility of customer context.                                  |
| **How It Works**     | Operates as a background or embedded panel analyzing conversation streams (voice or chat) and surfacing insights.                                  | Provides multi-channel access, ticket management, and embeds Agent Assist widgets for live AI recommendations.                                   |
| **Typical Features** | - Intent/sentiment detection<br>- Knowledge article surfacing<br>- Suggested responses<br>- Auto-summarization<br>- Compliance and script guidance | - Conversation history<br>- CRM/ticket integration<br>- Manual control and workflow management<br>- Access to AI-powered tools from Agent Assist |
| **User Impact**      | Enhances accuracy and reduces cognitive load for agents.                                                                                           | Provides central workspace and channel control, ensuring smooth handling of interactions.                                                        |
| **Integration**      | Embeds into Kore.ai’s or third-party desktops (Salesforce, Zendesk, Genesys).                                                                      | Hosts Agent Assist modules and connects with back-end systems.                                                                                   |
| **Outcome**          | Enables smarter, faster resolutions through AI guidance.                                                                                           | Delivers a unified operational view for the agent, combining human empathy with AI assistance.                                                   |

**Summary:** *Agent Assist* is the **AI-driven guidance layer**, while *Agent Desktop* is the **agent’s working environment**. When combined, they form an intelligent agent experience—AI listens, guides, and automates, while the desktop delivers the workspace and communication tools.

<!-- ********************* -->

# Can Agent Assist Exist Without Agent Desktop?

<!-- ********************* -->

Kore.ai allows **Agent Assist (Agent AI)** to operate **independently of the Agent Desktop**. While both are complementary, they are not mutually dependent.

### ✅ When Agent Assist Runs Without Agent Desktop

* Agent Assist can be **embedded inside third‑party CRMs or CCaaS systems** such as Salesforce, Zendesk, Genesys Cloud, or NICE.
* In these scenarios, agents continue using their **native desktop or console**, but Kore.ai’s **AI‑assist panel** appears as an embedded widget or sidebar.
* The AI layer still performs:

  * Real‑time intent and sentiment detection from chat or voice streams.
  * Surfacing of knowledge articles and scripted guidance.
  * Generation of summaries and automated CRM field updates.
* Kore.ai interacts with external platforms through **API connectors and SDKs** to feed insights and actions directly into the agent’s interface.

### ⚙️ When Agent Desktop Is Included

* Enterprises that **adopt Kore.ai as a full contact‑center solution** can enable the native **Agent Desktop** module.
* In this case, the Agent Desktop becomes the **primary workspace**, and **Agent Assist** is **built‑in**, offering unified functionality for chat, voice, and social channels.

### 🔍 Key Takeaways

| **Scenario**                                               | **Agent Assist Availability**   | **Agent Desktop Presence** |
| ---------------------------------------------------------- | ------------------------------- | -------------------------- |
| Embedded in existing CRM/CCaaS (e.g., Salesforce, Genesys) | ✅ Yes — runs as an AI panel     | ❌ No — uses third‑party UI |
| Using Kore.ai full contact‑center suite                    | ✅ Yes — fully integrated        | ✅ Yes — native workspace   |
| Knowledge‑only or AI guidance use case                     | ✅ Yes — standalone assist layer | ❌ Optional                 |

**Summary:** *Agent Assist* can operate as a **standalone AI layer** within other agent systems, or as an **integrated feature inside Kore.ai’s own Agent Desktop**. Agents do not always have the Kore.ai desktop, but they can always leverage Kore.ai’s cognitive assistance wherever they work.

<!-- ********************* -->

# Understanding CCaaS (Contact Center as a Service)

<!-- ********************* -->

**CCaaS (Contact Center as a Service)** is a cloud-based model that delivers contact center functionality — including voice, chat, email, routing, analytics, and workforce management — through subscription-based or hosted platforms. Kore.ai integrates deeply with CCaaS systems to extend them with conversational AI and automation.

### 🧩 What CCaaS Provides

* **Core Contact Handling**: Manages inbound and outbound calls, chat, and digital interactions.
* **Routing & Queues**: Skill-based routing, queue assignment, and escalation flows.
* **Workforce Management (WFM)**: Agent scheduling, adherence tracking, and capacity forecasting.
* **Reporting & Analytics**: Metrics such as AHT (Average Handle Time), FCR (First Contact Resolution), and CSAT (Customer Satisfaction).
* **CRM Integration**: Seamless handoff of customer data and tickets between front-line interactions and back-end systems.

### ⚙️ How Kore.ai Integrates with CCaaS

| **Integration Point**          | **Kore.ai Role**                                                                                       |
| ------------------------------ | ------------------------------------------------------------------------------------------------------ |
| **Pre-Agent Automation**       | Handles self-service and intent capture before routing to a live agent.                                |
| **AI Routing**                 | Uses conversational context, sentiment, and metadata to improve routing accuracy.                      |
| **Agent Assist Layer**         | Embeds as a sidebar or widget inside CCaaS consoles (e.g., Genesys Cloud, NICE CXone, Amazon Connect). |
| **Post-Interaction Summaries** | Auto-generates summaries, sentiment scores, and compliance notes for the CCaaS system.                 |
| **Knowledge Integration**      | Exposes enterprise KBs and RAG-based answers directly within CCaaS agent consoles.                     |

### 💡 Examples of CCaaS Platforms Supported by Kore.ai

* **Genesys Cloud CX** — integrated bot connector and routing handoff.
* **Amazon Connect** — telephony and transcription integration via Voice Gateway.
* **NICE CXone** — embedding of Agent Assist and analytics connectors.
* **Cisco Webex Contact Center** — integration for chat and voice automation.
* **Five9** — integration for both self-service and assisted-service workflows.

### 🧠 Summary

> CCaaS provides the **operational foundation** for contact handling and routing, while Kore.ai adds the **intelligence layer** — delivering automation, contextual understanding, and AI-driven guidance across the same ecosystem.

---

# Understanding Transcription in Kore.ai

<!-- ********************* -->

**Transcription** in Kore.ai refers to the automated process of converting **spoken language (voice calls or IVR interactions)** into text in real time or post-call. It serves as a critical bridge between **voice channels** and **AI-driven understanding**.

### 🔍 How Transcription Works

1. **Speech Capture** – During a live call, the Kore.ai Voice Gateway or CCaaS partner streams audio to a speech engine (ASR – Automatic Speech Recognition).
2. **Real-time Conversion** – The ASR component transcribes voice into text, enabling the same NLU pipeline used for chat interactions.
3. **Semantic Processing** – Once text is available, Kore.ai’s NLU engine analyzes intent, sentiment, and entities.
4. **Storage & Analytics** – Transcriptions are securely stored for summarization, quality analysis, and training.

### ⚙️ Kore.ai Capabilities

* **Multi-engine support:** Uses native ASR or integrates with external engines (Google, AWS, Azure).
* **Real-time intent tagging:** Allows AI to interpret live voice calls.
* **Post-call transcription:** Enables after-call work summaries and Quality AI analytics.
* **Language & accent adaptation:** Supports global deployment.

### 🧠 Why It Matters

* Converts voice to data for AI understanding.
* Enables consistent analytics across voice and text channels.
* Reduces manual documentation through automatic call summaries.

---

# Understanding Next Best Action (NBA) in Kore.ai

<!-- ********************* -->

**Next Best Action (NBA)** in Kore.ai refers to the system’s ability to **recommend the most contextually relevant action, message, or workflow** to either a user or an agent during an interaction.

### 🧩 How NBA Works

1. **Context Collection:** Kore.ai gathers information from conversation history, CRM records, and detected intent or sentiment.
2. **AI Evaluation:** ML models and business rules evaluate available actions (e.g., upsell, resolve, verify, escalate).
3. **Recommendation Generation:** The AI suggests the best action to take next—such as providing an answer, routing to an agent, or launching a workflow.
4. **Execution:** The agent or bot executes the suggestion, and results feed back into analytics for optimization.

### ⚙️ Kore.ai Capabilities

* **Real-time agent guidance:** Surface recommended responses or workflows in Agent Assist.
* **Customer experience optimization:** Suggests proactive offers or follow-ups for users.
* **Adaptive learning:** Improves recommendations over time based on feedback.

### 🧠 Why It Matters

* Increases resolution speed and personalization.
* Improves agent efficiency by minimizing decision fatigue.
* Ensures consistent handling of complex or regulated scenarios.

---

# Understanding SmartAssist in Kore.ai

<!-- ********************* -->

**SmartAssist** is Kore.ai’s pre-packaged contact center solution that combines conversational AI, automation, and agent assist capabilities into a single ready-to-deploy platform. It is designed to accelerate deployment for enterprises that want a complete AI-powered contact center without building each module from scratch.

### 🔧 Core Features

* **Pre-built Self-Service Bots:** Ready templates for common contact center use cases (banking, insurance, telecom, etc.).
* **Agent Assist Integration:** Embeds Kore.ai’s Agent AI features to provide real-time guidance, knowledge retrieval, and next-best actions.
* **Omnichannel Experience:** Unified voice, chat, and digital channels, allowing seamless switching between self-service and assisted-service.
* **Routing & Queueing:** Integrates with CCaaS platforms for intelligent handoff, or uses built-in routing logic.
* **Analytics & Quality AI:** Monitors performance, compliance, and sentiment across every interaction.

### ⚙️ How It Differs from XO Platform

| **Aspect**      | **SmartAssist**                                                  | **XO Platform**                                              |
| --------------- | ---------------------------------------------------------------- | ------------------------------------------------------------ |
| **Purpose**     | Turnkey contact center solution.                                 | Developer and enterprise platform for custom AI experiences. |
| **Deployment**  | Ready-to-use; minimal setup.                                     | Requires configuration and design of bots and flows.         |
| **Audience**    | Contact center operations and customer support teams.            | Developers, architects, and enterprise AI teams.             |
| **Integration** | Pre-integrated with Agent Assist, Quality AI, and Voice Gateway. | Integrates via APIs for more flexible use cases.             |

### 🧠 Why It Matters

* Offers a **faster path to production** with pre-configured best practices.
* Ensures **tight integration** across voice, chat, and agent workflows.
* Allows organizations to leverage Kore.ai’s **full AI stack** (NLU, Knowledge AI, Quality AI, and Voice Gateway) in one cohesive solution.

---

# Is SmartAssist Optional or a Simpler Development Platform?

<!-- ********************* -->

**SmartAssist** is *not* a replacement for the XO Platform, nor is it merely a simpler development tool. Instead, it’s an **optional, pre-packaged layer built on top of the XO Platform** that simplifies contact center deployment.

### ⚙️ Relationship Between SmartAssist and XO Platform

| **Aspect**          | **SmartAssist**                                                                         | **XO Platform**                                                                         |
| ------------------- | --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Role**            | Pre-built, ready-to-use contact center solution.                                        | Foundational platform for developing, training, and managing AI agents.                 |
| **Customization**   | Offers limited customization—mainly configuration of pre-existing components.           | Fully customizable with low-code tools for bespoke automation and multi-channel design. |
| **Target Audience** | Business and operations teams that want speed-to-value and pre-integrated modules.      | Developers, architects, and enterprise AI teams who design end-to-end experiences.      |
| **Dependencies**    | Runs *on top of* the XO Platform; uses its NLU, Knowledge AI, and integration services. | The base platform that supports SmartAssist and other Kore.ai modules.                  |

### 🧭 Summary

> SmartAssist is **optional**. It is best viewed as a **turnkey contact center product** built using the XO Platform. Organizations can either use SmartAssist for faster implementation or directly develop within the XO Platform for maximum flexibility.

---

# Understanding BotKit SDK in Kore.ai

<!-- ********************* -->

The **BotKit SDK** is Kore.ai’s developer toolkit that enables advanced customization, integration, and extension of bots built on the XO Platform. It provides **server-side APIs and libraries** that developers can use to handle business logic, third-party integrations, and event-driven interactions outside the visual builder.

### ⚙️ Key Features

* **Server-Side Execution:** Lets developers write Node.js-based code to execute logic beyond the platform’s low-code limits.
* **Event Handlers:** Supports triggers such as message received, dialog start/end, task execution, and webhook events.
* **Custom Business Logic:** Enables validation, computation, or database lookups during conversations.
* **External Integrations:** Provides APIs to connect with enterprise systems (CRM, ERP, ticketing) securely.
* **Extensibility:** Developers can use the SDK to add middleware functions, transform responses, or inject context before replies.

### 🧩 Typical Use Cases

| **Use Case**   | **Example**                                                   |
| -------------- | ------------------------------------------------------------- |
| Data Fetch     | Retrieve real-time inventory data from ERP before responding. |
| Validation     | Check account eligibility or policy rules via external API.   |
| Logging        | Capture custom metrics or events for analytics.               |
| Custom Actions | Trigger robotic process automation (RPA) or workflow systems. |

### 🔧 Deployment Model

* Runs as a **microservice** or **Node.js application** hosted in the enterprise’s environment or Kore.ai’s cloud.
* Communicates securely with the XO Platform using REST APIs and authentication tokens.
* Scales independently for high concurrency environments.

### 🧠 Why It Matters

* Provides **developer-level control** for complex enterprise needs.
* Extends Kore.ai’s low-code environment with **programmable flexibility**.
* Bridges the gap between conversational design and enterprise-grade application logic.

---

# How BotKit SDK Differs from XO Platform

<!-- ********************* -->

While both the **XO Platform** and **BotKit SDK** are developer tools within Kore.ai’s ecosystem, they serve different purposes and complement each other.

### ⚙️ Key Distinctions

| **Aspect**                | **XO Platform**                                                                        | **BotKit SDK**                                                                      |
| ------------------------- | -------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| **Purpose**               | Low-code visual environment for designing, training, and managing conversational bots. | Developer SDK for extending bot functionality through custom code and integrations. |
| **Audience**              | Business users, bot designers, and solution architects.                                | Backend developers and integration engineers.                                       |
| **Interface**             | Web-based visual builder (drag-and-drop dialog design, training UI).                   | Code-based toolkit (Node.js, REST APIs).                                            |
| **Execution Environment** | Runs within Kore.ai’s hosted platform.                                                 | Runs as a separate service that communicates with the XO Platform via APIs.         |
| **Extensibility**         | Supports modular design, intents, entities, dialog tasks, and built-in integrations.   | Enables event-driven logic, custom middleware, and fine-grained system control.     |
| **Customization Level**   | High (within visual boundaries).                                                       | Very high (unlimited via code).                                                     |
| **Maintenance**           | Managed entirely through the XO Platform interface.                                    | Requires hosting and lifecycle management by the developer or enterprise.           |

### 🧩 Relationship Summary

* The **XO Platform** is the **core foundation** where bots are created, trained, and orchestrated.
* The **BotKit SDK** acts as an **extension framework** for advanced use cases that need logic beyond the built-in visual tools.
* Most enterprises use both together: XO for design and management, BotKit SDK for backend intelligence and integrations.

---

# Understanding AI for Work in Kore.ai

<!-- ********************* -->

**AI for Work** is Kore.ai’s suite of enterprise productivity solutions that apply conversational and generative AI across everyday workplace tools and business workflows. It extends the capabilities of the XO Platform beyond customer service and contact centers, embedding intelligence into enterprise operations.

### ⚙️ Core Purpose

AI for Work helps employees automate tasks, retrieve knowledge, and perform actions conversationally — using the same NLU and dialog orchestration engine that powers customer-facing bots.

### 🧩 Key Features

* **Conversational Workflows:** Enables employees to interact with enterprise systems (CRM, HR, ITSM, ERP) using natural language.
* **Workplace Assistants:** Pre-built virtual assistants (HR Assist, IT Assist, Sales Assist, etc.) streamline common requests like leave management, ticket creation, or data lookup.
* **Generative AI & Summarization:** Summarizes documents, meetings, and knowledge content to enhance productivity.
* **Omnichannel Integration:** Works across chat platforms such as Microsoft Teams, Slack, or Web interfaces.
* **Automation Hooks:** Integrates with RPA, APIs, and process automation tools for end-to-end task execution.

### 🔗 Relationship with Conversational AI

| **Aspect**            | **Conversational AI**                               | **AI for Work**                                                                             |
| --------------------- | --------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **Audience**          | External users/customers.                           | Internal employees and enterprise teams.                                                    |
| **Primary Use Case**  | Customer engagement, self-service, and support.     | Enterprise productivity, task automation, and knowledge retrieval.                          |
| **Underlying Engine** | XO Platform (NLU, Knowledge AI, Dialog Management). | Built on the same XO Platform, leveraging its bots and workflows for employee interactions. |
| **Integration**       | CCaaS, CRM, and customer systems.                   | Workplace tools (Teams, Slack, ServiceNow, Workday, Salesforce).                            |

### 🧠 Why It Matters

* Extends conversational AI into **enterprise productivity domains**.
* Brings **the same AI models** used for customers to assist employees internally.
* Improves efficiency, reduces context-switching, and accelerates knowledge access.

> **In essence:** *AI for Work* merges Kore.ai’s conversational AI framework with internal automation and collaboration tools, turning every employee interaction into an intelligent, natural-language experience.

---

# Supporting Components and Architecture Terms

<!-- ********************* -->

| **Term**                        | **Definition / Role**                                                                                    |
| ------------------------------- | -------------------------------------------------------------------------------------------------------- |
| **Bot / Virtual Assistant**     | The conversational interface created in the XO Platform to interact with users.                          |
| **Channel Connector**           | Integration endpoint connecting Kore.ai bots to specific channels (Web SDK, WhatsApp, Slack, IVR, etc.). |
| **Dialog Node**                 | A single unit within a Dialog Task representing a question, logic branch, or API call.                   |
| **Webhook / API Node**          | Executes external API requests within a dialog for data retrieval or updates.                            |
| **Queue / Skill**               | Attributes defining agent expertise or language used in routing rules.                                   |
| **Utterance**                   | Any phrase typed or spoken by a user; used for training intents.                                         |
| **Training Data**               | Set of sample utterances and entities used to train the bot’s ML models.                                 |
| **Workflow Automation**         | Tasks or sequences triggered by bot actions to perform back-end system updates.                          |
| **Connector / Integration Hub** | Pre-built integrations to CRM, ERP, WFM, and ticketing systems.                                          |
| **Security & Compliance**       | Kore.ai adheres to SOC 2, ISO 27001, HIPAA, and supports data masking, RBAC, and on-prem deployments.    |

---

<!-- ********************* -->

# References

<!-- ********************* -->

* [Kore.ai Platform Overview](https://kore.ai/platform/) — Official product overview and module list.
* [Kore.ai Documentation Portal](https://docs.kore.ai/) — Complete developer documentation and glossary.
* [Kore.ai XO Platform Architecture](https://docs.kore.ai/docs/platform/overview/platform-architecture/) — Architecture diagrams and platform components.
* [Kore.ai Agent AI](https://kore.ai/platform/agent-ai/) — Details on agent-assist features and real-time recommendations.
* [Kore.ai Knowledge AI](https://kore.ai/platform/knowledge-ai/) — Knowledge management and retrieval module description.
* [Kore.ai Voice Gateway](https://kore.ai/platform/voice-gateway/) — IVR and telephony integration capabilities.
* [Kore.ai Quality AI](https://kore.ai/platform/quality-ai/) — Conversation analytics, scoring, and insights.

*Link validation date: November 2, 2025.*

<!-- ********************* -->
# Understanding How LLM Chats, Agents, and Tools Interact
<!-- ********************* -->

> 1. **Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.**
> 2. This is an early draft. Going through each section and manually updating it correctly
> 4. Will be ready this weekend. 6/6/2025: Satya Komatineni
> 5. First adjustments complete. 6/8/2025
> 6. Still an incomplete research. At least a starting point.

This article explores the relationships and boundaries between LLM chats, agents, and tools, addressing a series of practical design and architectural questions. These questions were asked interactively and are restated here to frame the discussion:

* How do LLM chats and agents interact or not interact?
* How are chats used as interfaces to agentic systems?
* How are agents exposed to the chat host?
* Can these agents run out of process? How do you share the context of the conversation with sub-agents?
* How do today’s chat systems normally share context? Does Google A2A solve this formally?
* Do chats assume that (MCP) tools are stateless?
* When an agent is implemented as a tool and expects state, do you pass the chat history in every call or build a protocol on top?
* How do ChatGPT or Claude integrate with MCP tools? Do they pass history or summary to the tool?

<!-- ********************* -->
# LLM Chats vs Agents: What’s the Difference?
<!-- ********************* -->

When folks say LLM chats **can** use Agents, what does that mean? Agents have their own behavior. Do chats share state? or is it always stateless, for now in chats like ChatGPT or Claude?

To understand or explore that, first see how the industry has seen Chats and Agents so far.

## Chats

LLM chats are typically designed as user-facing, open-ended conversation interfaces. They rely on real-time user prompts to generate answers but are stateless by default. 

Chats lack autonomy

1. Doesn’t have memory or multi-step planning (unless added manually).
2. Doesn’t use tools unless explicitly prompted.
3. Doesn't "act" without user direction.
4. They lack role specialization like a planner, executor, researcher etc.
5. Chats don't enforce work flows, they are free form

## Agents
In contrast, agents are goal-oriented orchestrators. They can reason, use tools, plan multiple steps, and persist memory between calls. Agents often embed LLMs for natural language understanding but maintain structure and autonomy.

Agents typically:

1. Define roles (planner, executor, retriever).
2. Manage tools (APIs, databases, file systems).
3. Maintain persistent memory or context beyond a single prompt.
4. Operate semi-autonomously or fully autonomously.
5. Can plan and execute sequences without continuous human input.

**Example:**

A user asks an LLM, “Can you summarize this article?”

→ The LLM in chat just responds.

→ An agent might:

1. Detect the task.
2. Retrieve the article from a URL.
3. Use a summarization tool.
4. Save the result to a notes database.

## Summary table

| Feature                   | LLM Chat                          | Agent                                  |
| ------------------------- | --------------------------------- | -------------------------------------- |
| Role                      | User-driven conversation          | Goal-driven orchestrator               |
| Autonomy                  | None or user-triggered            | Semi- or fully-autonomous              |
| Tool use                  | Manual or limited                 | Integrated, dynamic                    |
| Memory                    | Temporary or shallow              | Persistent, often structured           |
| Multi-step planning       | Rare (unless explicitly prompted) | Core feature (with optional recursion) |
| Multi-agent collaboration | No                                | Yes                                    |


<!-- ********************* -->
# Chats as Interfaces to Agent Systems
<!-- ********************* -->

Chat interfaces can, and now starting to, serve as thin front-ends to complex agent systems. The user sees a conversational UI, but the backend may involve planners, retrievers, and executors.

For example, when a user asks for a draft email in Microsoft Copilot, the chat window triggers agents that fetch content, summarize past communications, and generate a reply—all behind the scenes. This separation allows user-friendly interaction on top of structured orchestration.

<!-- ********************* -->
## How could this work, an example
<!-- ********************* -->

Take a customer service chatbot:

You (the user) type a question: **“Can you find the cheapest flight to Chicago?”**

You see a simple response.

But behind the scenes:

1. The system parses your intent.
2. Calls an API to search flights.
3. Ranks and filters the results.
4. Formats a friendly answer.

That "system" handling those tasks is the agent. The chat interface is just the surface.

<!-- ********************* -->
## MS Word as a host: Email example
<!-- ********************* -->

You ask in Word: “Summarize the last five emails and create a draft reply.”

Agent:

1. Queries Outlook for emails.
2. Summarizes them with an LLM.
3. Returns to the word host

Copilot Word chat:

1. Drafts a response in Word.
2. You just see a chat reply, but the agent did several tasks in sequence.

<!-- ********************* -->
## Notion AI example
<!-- ********************* -->
You chat: “Turn these notes into a project plan.”

Agent:

1. Analyzes structure.
2. Applies templates.
3. Inserts sub-tasks.

Chat Output: a formatted, multi-part document.

<!-- ********************* -->
# How Agents Are Exposed to the Chat Host
<!-- ********************* -->

Agents are made accessible to chat hosts through a few common mechanisms:

* **Function/tool schema calls** (e.g., OpenAI function calling)
* **Router/planner dispatchers** (e.g., LangChain RouterChain)
* **Middleware orchestrators** (e.g., Autogen or CrewAI)
* **MCP Tool calls** to external agent services
* **Prompt-level embeddings** (e.g., ReAct-style agents embedded in LLM context)

Each of these mechanisms determines how agents receive inputs and return outputs, and whether they support concurrent sessions or tool composition.

---

<!-- ********************* -->
# Running Agents Out-of-Process and Sharing Context
<!-- ********************* -->

Agents can run as MCP Tools. Although there are emerging standards like Google A2A.

Common patterns for sharing context:

* Passing conversation history or summary on every call
* Storing shared memory in Redis or a vector store
* Middleware that injects memory into the agent's prompt
* Using session IDs to track and retrieve task-specific state

<!-- ********************* -->
# How Most Chat Systems Handle Context
<!-- ********************* -->

So in systems like Chats with tools, memory is assembled and passed into each call behind the scenes.

* A limited window of previous chat turns is included.
* Summaries are generated for older turns.
* Function calls receive context only if explicitly passed.


<!-- ********************* -->
# Google A2A and Formal Context Management
<!-- ********************* -->

Google’s Agents-to-Agents (A2A) framework formally addresses context as a first-class object. Agents operate with shared structured context:

* The execution environment manages the context state.
* Each agent appends or reads from this context.
* Context is versioned, observable, and isolated.

This enables modular agent orchestration with clean memory separation.

**From Google’s A2A whitepaper and talks:**

1. Agents interact through a shared structured context managed by the execution environment.
2. Context is versioned and scoped.
3. Agents don’t operate on raw prompts—they receive structured inputs and return structured outputs.
4. There’s a separation between agent logic and context plumbing.

This allows:

1. Consistent, clean state across agents
2. Composability (agents can call other agents easily)
3. Observability (since context is structured)

<!-- ********************* -->
## A quick comparison
<!-- ********************* -->


| Feature                  | Traditional Chat w/ Tools  | Google A2A                             |
| ------------------------ | -------------------------- | -------------------------------------- |
| Context format           | Raw text + manual metadata | Structured context object              |
| Context memory           | Passed per prompt          | Persisted in orchestrator              |
| Inter-agent coordination | Ad hoc via prompting       | Managed via execution engine           |
| Visibility and tracing   | Limited to logs            | Full visibility over context evolution |
| Agent invocation         | Prompt + tool schema       | Structured agent API                   |


<!-- ********************* -->
## Summary of A2A
<!-- ********************* -->

1. Most chats today manually manage context, passing summaries or windowed history.
2. Google A2A solves this formally using a shared, structured context object.
3. This allows robust inter-agent collaboration, memory, and modularity.


<!-- ********************* -->
# Are Tools Assumed to Be Stateless?
<!-- ********************* -->

Yes. In OpenAI, Claude, and LangChain, tools are assumed to be stateless. Each invocation is a pure function with no memory of prior calls.

If a tool requires state:

* It must receive context or tokens explicitly.
* Or it should be promoted to a stateful agent.

<!-- ********************* -->
## Typical tool design
<!-- ********************* -->

Tools are typically designed as:

1. Pure functions: Input → Output
2. No internal memory between invocations
3. No assumption of conversation continuity

Some example tools are:

1. A calculator
2. A weather API
3. A database lookup
4. A long-running subprocess

Some systems where this approach is demonstrated:

1. OpenAI's function calling / tools
2. Anthropic's tool use (Claude)
3. Google Gemini functions
4. LangChain agents
5. Autogen tool wrappers

<!-- ********************* -->
## Some arguments to be stateless
<!-- ********************* -->
{
  "tool_name": "search_flights",
  "arguments": { "from": "JAX", "to": "ORD", "date": "2025-06-12" }
}

The tool runs, returns results, and forgets the call.

**The LLM is responsible for remembering:

1. What was asked before
2. What the result meant
3. Whether to call it again with different arguments

<!-- ********************* -->
## Some arguments to be stateless
<!-- ********************* -->

| Reason              | Description                                                            |
| ------------------- | ---------------------------------------------------------------------- |
| **Simplicity**      | Stateless tools are easier to compose, call, and debug.                |
| **Concurrency**     | Multiple agent threads or LLMs can use them in parallel.               |
| **Reproducibility** | Same input → same output = deterministic behavior.                     |
| **Isolation**       | Prevents one user session from corrupting another.                     |
| **Scaling**         | Easier to horizontally scale when no shared session state is required. |


<!-- ********************* -->
# Stateful Tools: Passing History vs an Agent Protocol
<!-- ********************* -->

Sometimes, tools do need to manage ongoing state:

1. Multi-step workflows
2. Background tasks (e.g., report generation)
3. Sessions (e.g., DB transaction, simulation engine)
4. Progressive refinement (e.g., interactive code agent)

When an agent is implemented as a tool but requires state, two main approaches exist:

1. **Pass chat history or summary** in each call (easy, compatible with function-calling APIs)
2. **Create a protocol** with methods like `start_session`, `set_context`, `execute`, `end_session` (scalable, modular, formal)

The second approach is more robust and aligns with how frameworks like Autogen or A2A manage sessions.

<!-- ********************* -->
## An example invocation of a stateful agent
<!-- ********************* -->

{
  "agent_id": "planner-agent",
  "session_id": "sess-87e2a1",
  "user_id": "user-123",
  "history": [
    {
      "role": "user",
      "content": "Can you help me book a flight to San Francisco?"
    },
    {
      "role": "assistant",
      "content": "Sure! What date are you planning to travel?"
    },
    {
      "role": "user",
      "content": "This Friday."
    },
    {
      "role": "assistant",
      "content": "Got it. Do you prefer morning or evening flights?"
    },
    {
      "role": "user",
      "content": "Morning is better."
    }
  ],
  "current_message": {
    "role": "user",
    "content": "Also I might need a return flight."
  },
  "timestamp": "2025-06-06T20:30:00Z"
}

<!-- ********************* -->
## Pros and cons of passing history with every call
<!-- ********************* -->

Pros

1. Easy to implement, no new protocol
2. Compatible with OpenAI, Claude, etc.
3. Stateless tools remain scalable

Cons

1. Payloads can become large
2. Repetition of same memory can be inefficient
3. Agents cannot preserve fine-grained internal state (e.g., internal steps, scratchpads)

<!-- ********************* -->
## How various framesworks treat tools
<!-- ********************* -->

Most don't assume state by default, but some support stateful behavior via conventions:

| Framework       | Tool Statefulness           | How it’s handled                        |
| --------------- | --------------------------- | --------------------------------------- |
| **OpenAI GPTs** | Stateless                   | You simulate state via memory tools     |
| **LangChain**   | Stateless                   | Use `RunnableWithConfig`, memory stores |
| **Autogen**     | Supports sessions           | Agents can persist tool history         |
| **CrewAI**      | Tools are stateless         | State passed via shared memory dict     |
| **Google A2A**  | Supports structured context | Context carries past tool usage         |

<!-- ********************* -->
## Summary
<!-- ********************* -->

1. Yes, tools are assumed stateless in most chat systems.
2. This simplifies design, but pushes responsibility for context and reasoning back to the LLM or orchestrator.
3. When state is needed, systems use session tokens, shared memory, or elevate the tool into an agent.

<!-- ********************* -->
## A formal protocol approach instead
<!-- ********************* -->

This approach is more formal when building stateful agent-tools.

An example conversation

```json
// Step 1: Start session
{ "tool": "stateful_agent", "arguments": { "action": "start_session" } }

// Step 2: Set context
{ "tool": "stateful_agent", "arguments": { "action": "set_context", "session_id": "abc123", "history": [...] } }

// Step 3: Invoke task
{ "tool": "stateful_agent", "arguments": { "action": "execute", "session_id": "abc123", "task": "summarize_report" } }

// Step 4: End session
{ "tool": "stateful_agent", "arguments": { "action": "end_session", "session_id": "abc123" } }
```

Pros

1. Supports long-term state or task-specific memory
2. Can manage complex workflows, progressive reasoning
3. Cleaner separation of session lifecycle

Cons

1. Requires orchestration logic to manage session tokens
2. Needs state store (Redis, DB, in-memory map)
3. Not directly compatible with OpenAI tools unless wrapped or abstracted

## A comparison table

| Strategy                        | Chat History Per Call   | Session Protocol                     |
| ------------------------------- | ----------------------- | ------------------------------------ |
| **Effort to Implement**         | Low                     | Moderate–High                        |
| **State Fidelity**              | Shallow (summary-based) | High (true memory)                   |
| **Scalability**                 | OK for short sessions   | Better for long, multi-step sessions |
| **Compatibility (e.g. OpenAI)** | Fully compatible        | Needs adaptation or abstraction      |
| **Ideal Use Case**              | Lightweight agents      | Multi-turn or persistent agents      |

## How to choose

**Use history-per-call for:**

1. Quick LLM agent tools
2. Stateless toolchains
3. Tool use with OpenAI/Claude plugins
   
**Use session protocol for:**

1. Multi-turn agents with internal memory
2. Simulators, planners, background task managers
3. Advanced frameworks like Autogen, A2A, or in-house middleware

## Possibility for a hybrid approach

Some systems combine both:

1. Session ID = lightweight handle
2. Still pass short context slices
3. Background memory manager maintains deeper state


<!-- ********************* -->
# ChatGPT and Claude with MCP Tools
<!-- ********************* -->

This is very new for these tools.

More research is needed on my part.

One possible approach is to have the stateless tool explicitly define the context parameters either as full history or summary.

<!-- ********************* -->
# References
<!-- ********************* -->

1. **[Google A2A: Agents that Communicate and Collaborate](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/)** — Describes Google's formal agent architecture with structured context, session state, and orchestration.

2. **[OpenAI Function Calling](https://platform.openai.com/docs/guides/function-calling)** — Explains how to integrate stateless tools into GPT-based chat systems via structured JSON functions.

3. **[Anthropic Claude Tool Use](https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/overview)** — Covers how Claude uses function calls and passes arguments to external tools.

4. **[LangChain Agents Documentation](https://python.langchain.com/api_reference/core/agents.html)** — Discusses how agents, tools, and context interact in LangChain, including routing, memory, and tool exposure.

5. **[Autogen by Microsoft](https://github.com/microsoft/autogen)** — Shows how multi-agent collaboration is structured, including context passing, history retention, and task planning.

6. **[Model Context Protocol (MCP) Specification](https://modelcontextprotocol.io/specification/2025-03-26)** — Defines the authoritative protocol requirements for integrating LLM applications with external data sources and tools.

7. **[Talkdesk Blog: What are AI agents? Benefits, types, and use cases](https://www.talkdesk.com/blog/ai-agents/)** — Explores the differences between chatbots and AI agents, highlighting their capabilities in handling complex queries and providing comprehensive conversation summaries.

8. **[LangChain Documentation: How to Add Tools to Chatbots](https://python.langchain.com/docs/how_to/chatbots_tools/)** — Provides a guide on creating conversational agents that can interact with other systems and APIs using tools, emphasizing the integration of chat history and memory into chatbots.

9. **[Neudesic Blog: AI Chatbots vs AI Agents](https://www.neudesic.com/blog/ai-chatbots-vs-ai-agents/)** — Discusses the key differences between AI chatbots and AI agents, focusing on their respective capabilities in handling routine tasks versus complex problem-solving.

10. **[FlowHunt Blog: Create AI Chatbot with AI Agents](https://www.flowhunt.io/blog/create-ai-chatbot-with-ai-agents/)** — Offers a step-by-step guide on building AI chatbots using tool-calling agents, detailing how to integrate various tools and manage chat history for enhanced functionality.

# GenAI Architecture Guidance for Enterprise Teams

8/31/2025, Satya Komatineni

A set of principles for GenAI Architectures in an Enterprise.

These principles hope to achieve

1. Simpler architectures through horizontal UXs, MCP servers, and English Agents
2. Get greater value from smaller efforts: **An enterprise AI command shell** that allows one to script enterprise functions
3. Spend investments that enable 1 and 2

Note: This architecture is based on current trends in the industry. It may evolve, like all things. This is very much inline with how various popular GenAI vendors are approaching their enterprise offerings.

**TL;DR:**

**There is an opportunity to dramatically increase the value of GenAI in Enterprises because of MCPs and horizontal GenAI UX tools**

# Principles

1. Maximally use horizontal GenAI tools to achieve business goals
2. Spend efforts to enhance the horizontal tools for greater capabilities [Unsless you bought a vendor like Microsoft, OpenAI, or AWS]
3. Some example horizontal tools are: Chats, MS Teams, Excel, SharePoint
4. Use MCP servers to enhance the power of horizontal tools
5. Use MCP Servers for both reads and writes
6. Enhance the infrastructure to write and manage MCP servers
7. Use Prompts and Agents (written in English) that run on MCP servers and exploited by the horizontal UX
8. Enhance the infrastructure to write and manage Agents
9. Finally solve your problems with: App -> Agents -> MCP Servers [Where the App often is horizontal and need not be coded.]
10. Think: could you have solved this problem with MCPs, Horizontal UX, and Prompts (also called Agents)

# Where is the more valuable work in GenAI in the begining phases?

1. Strengthening horizontal tools
2. Fully functional test beds for MCP Servers and Agents
3. Strengthening MCP production infrastructure
4. Strengthening Agent production infrastructure
5. Strengthening Semantic indexing infrastructure (Vectorization)
6. Stengthen prompt libraries

# Negative Patterns

1. Focus on "all genai efforts" with same focus
2. Spend energies and investments with equal value on all GenAI efforts. 
3. To think most applications as vertical when a horizontal ux with MCPs or Agents could "largely" solve the problem
4. Not fully realizing the power prompts when tools are available to the LLM

# Classes of use cases where horizontal ux is compelling

1. Knowledge repositories, Semantic indexing as MCP servers
2. Scoped, SQL Data exploration MCP servers
3. Excel  MCP servers to automate personal excel work that many teams do
4. Prompt MCP servers to stay versatile and nimble
5. Prompt MCP servers as agents
6. Domain MCP servers to use APIs to explore and answer domain questions
7. Domain MCP servers to use APIs to update work flows
8. Agents using MCP servers to do long running processes

# Types of GenAI Uxs

1. True Horizontal UX: Designed for multiple hybrid work loads. Examples are Chats, Github copilot, cursor. One application that does many things
2. Integrated UX: Where GenAI capabilities are integrated  into existing applications. Also known as "Meeting where you are". Examples are MS 365, Teams, MS Teams, Slack.
3. Custom UX: This is a completely vertical end user application that uses GenAI internally
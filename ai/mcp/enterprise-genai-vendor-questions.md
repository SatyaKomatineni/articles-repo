# Enterpise GenAI topics and questions for vendor research

What questions I should be asking an enterprise genai vendor?

# Questions

How is the vendor solving these for an enterprise

1. Document search
   1. How are you proposing to solve the search for unstructured documents?
   2. how do you scope the document search?
   3. what kind of ux is used?
   4. how do you index the files or documents?
2. Document summary and extraction
   1. Summaries
   2. data extraction
   3. composing new documents
3. Personal automation
   1. excel
   2. sharepoint
4. MCP, API, or custom connectors
   1. How are you exploring SQL or related data warehouses
   2. How are you using enterprise APIs for "Chat as an enterprise shell"
   3. Updating data with APIs
5. Agents
   1. Making them
   2. using them
   3. administration and monitoring them
   4. Long running workflows

# OpenAI Research questions

1. if I develop agents with agents sdk where do I run them?
2. What is sqlalchemy?
3. Compare and contrast with A2A
4. Now let me dwell into SP and ChatGPT. Tell me how it actually works
5. When i create a chatgpt-agent is it long running? or just a custom gpt?
6. Create some agents
7. Create some custom GPTs


## Key URLs

1. Agents SDK
2. Tool calling
3. Responses
4. ChatGPT - agents (ux)

## OpenAI key concepts

1. Responses API
2. Function calling API

## what to explore

1. data analysis
2. ux agents

## SP notes

1. read only
2. Can post individual urls
3. many file types supported
4. connectors including sp are per thread
5. ChatGPT makes live, per-thread queries against Microsoft’s own search index (Microsoft Graph / Microsoft Search)
6. Same is true with Box and OneDrive
7. For Google drive and github it uses its own synced semantic indexing
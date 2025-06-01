<!-- ********************* -->

# Simulating MCP Tool Handling in LLM Chat

<!-- ********************* -->

Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.

This document presents a structured test prompt and setup to simulate how an LLM chat (such as GPT-4) could handle questions by giving preference to predefined MCP-style tools before falling back to general knowledge. This is useful when experimenting with tool-like reasoning without requiring an actual MCP server.

<!-- ********************* -->

# Table of Contents

<!-- ********************* -->

* Simulated Tools
* System Prompt
* Sample Test Questions
* Final Prompt for Use
* Playground Config JSON
* References

<!-- ********************* -->

# Simulated Tools

<!-- ********************* -->

```
toolname: get_weather
    description: Retrieves the current weather for a given location.
    arguments:
        - location (string): The city and state or country.
    output: A JSON object with temperature, condition, and humidity.

toolname: define_term
    description: Provides a precise definition of a given term in plain English.
    arguments:
        - term (string): The term to define.
    output: A concise English definition.

toolname: convert_units
    description: Converts a numeric value from one unit to another.
    arguments:
        - value (float): The value to convert.
        - from_unit (string): The original unit.
        - to_unit (string): The target unit.
    output: A number and the converted unit in string form.

toolname: summarize_text
    description: Generates a short summary of a longer text.
    arguments:
        - text (string): The full text to summarize.
    output: A short summary in plain English.

toolname: get_stock_price
    description: Fetches the most recent stock price of a given company.
    arguments:
        - ticker (string): The company’s stock ticker symbol.
    output: A JSON object with current price, change %, and market status.
```

<!-- ********************* -->

# System Prompt

<!-- ********************* -->

> You are a helpful assistant with access to a set of predefined tools. When answering any question:
>
> 1. First, **attempt to match the user's request to a relevant tool**.
> 2. If a matching tool is found, **simulate a response** based solely on the tool’s metadata and input arguments.
> 3. If **no appropriate tool applies**, fall back to **general model knowledge**.
> 4. In every answer, indicate clearly whether:
>
>    * a tool was used, or
>    * the answer was generated using general model knowledge.

<!-- ********************* -->

# Sample Test Questions

<!-- ********************* -->

These questions are designed to test different pathways:

1. What’s the weather in Paris right now?
2. Define the term “epistemology.”
3. Convert 5 kilometers to miles.
4. What’s the stock price of Apple?
5. Summarize this text: "Artificial intelligence is a field of computer science that..."
6. Who won the 2020 US presidential election?
7. Can you translate 'hello' to French?
8. How humid is it in Mumbai today?

<!-- ********************* -->

# Final Prompt for Use

<!-- ********************* -->

Paste this into an LLM interface like the OpenAI Playground:

**System Prompt:**

> You are a helpful assistant with access to a set of predefined tools. When answering any question:
>
> 1. First, **attempt to match the user's request to a relevant tool**.
> 2. If a matching tool is found, **simulate a response** based solely on the tool’s metadata and input arguments.
> 3. If **no appropriate tool applies**, fall back to **general model knowledge**.
> 4. In every answer, indicate clearly whether:
>
>    * a tool was used, or
>    * the answer was generated using general model knowledge.

**User Context (Prepended):**

```
toolname: get_weather
    description: Retrieves the current weather for a given location.
    arguments:
        - location (string): The city and state or country.
    output: A JSON object with temperature, condition, and humidity.

toolname: define_term
    description: Provides a precise definition of a given term in plain English.
    arguments:
        - term (string): The term to define.
    output: A concise English definition.

toolname: convert_units
    description: Converts a numeric value from one unit to another.
    arguments:
        - value (float): The value to convert.
        - from_unit (string): The original unit.
        - to_unit (string): The target unit.
    output: A number and the converted unit in string form.

toolname: summarize_text
    description: Generates a short summary of a longer text.
    arguments:
        - text (string): The full text to summarize.
    output: A short summary in plain English.

toolname: get_stock_price
    description: Fetches the most recent stock price of a given company.
    arguments:
        - ticker (string): The company’s stock ticker symbol.
    output: A JSON object with current price, change %, and market status.
```

**Sample Question (example):**

> What’s the weather in Tokyo today?

<!-- ********************* -->
Fursther Tests
<!-- ********************* -->
Here are a few more questions for thorough testing.

<!-- ********************* -->
## Tool-Matching Questions
<!-- ********************* -->

These questions are expected to match the predefined tools directly.

### Matches `get_weather`

* What’s the weather in Tokyo today?
* Is it raining in New York?
* What’s the humidity like in Cairo?

### Matches `define_term`

* What does “resilience” mean?
* Define the term “entropy.”
* What is the meaning of “mollify”?

### Matches `convert_units`

* Convert 100 kilometers to miles.
* What is 32°F in Celsius?
* How many liters are in a gallon?

### Matches `summarize_text`

* Summarize this: “Climate change is accelerating due to greenhouse gas emissions...”
* Can you give me a short summary of this paragraph?
* Please condense this article into three sentences.

### Matches `get_stock_price`

* What is the current stock price of Google?
* Give me Apple’s market price now.
* How is TSLA performing today?

<!-- ********************* -->
## Fallback-to-General-Knowledge Questions
<!-- ********************* -->

These are not covered by the defined tools and should trigger general model reasoning.

### General Knowledge

* What is the capital of Finland?
* Who was the first person on the Moon?
* How many continents are there?

### Translation Requests

* How do you say “thank you” in Japanese?
* Translate “good night” to Spanish.

### Historical / Time-bound

* Who won the FIFA World Cup in 2014?
* What happened on 9/11?

<!-- ********************* -->
## Edge Cases & Ambiguity Tests
<!-- ********************* -->
These test the LLM’s reasoning to pick the best-fitting tool, or fail gracefully.
### Partial Overlap

* Tell me the temperature difference between Paris and Berlin. *(Should call `get_weather` twice?)*
* What is the meaning and origin of “serendipity”? *(Meaning matches, origin does not)*

### Multiple Matching Tools

* Give me the weather and summarize this report. *(Tests multitool parsing)*
* Define “calor” and translate it to English. *(Only one part matches a tool)*


<!-- ********************* -->
# Playground Config JSON
<!-- ********************* -->

Paste the following into the "Import configuration" option of the OpenAI Playground:

```json
{
  "model": "gpt-4-turbo",
  "system": "You are a helpful assistant with access to a set of predefined tools. When answering any question:\n\n1. First, attempt to match the user's request to a relevant tool.\n2. If a matching tool is found, simulate a response based solely on the tool’s metadata and input arguments.\n3. If no appropriate tool applies, fall back to general model knowledge.\n4. In every answer, indicate clearly whether:\n   - a tool was used, or\n   - the answer was generated using general model knowledge.",
  "messages": [
    {
      "role": "user",
      "content": "What’s the weather in Paris right now?"
    }
  ],
  "tools": [
    {
      "type": "function",
      "function": {
        "name": "get_weather",
        "description": "Retrieves the current weather for a given location.",
        "parameters": {
          "type": "object",
          "properties": {
            "location": {
              "type": "string",
              "description": "The city and state or country."
            }
          },
          "required": ["location"]
        }
      }
    },
    {
      "type": "function",
      "function": {
        "name": "define_term",
        "description": "Provides a precise definition of a given term in plain English.",
        "parameters": {
          "type": "object",
          "properties": {
            "term": {
              "type": "string",
              "description": "The term to define."
            }
          },
          "required": ["term"]
        }
      }
    },
    {
      "type": "function",
      "function": {
        "name": "convert_units",
        "description": "Converts a numeric value from one unit to another.",
        "parameters": {
          "type": "object",
          "properties": {
            "value": {
              "type": "number",
              "description": "The value to convert."
            },
            "from_unit": {
              "type": "string",
              "description": "The original unit."
            },
            "to_unit": {
              "type": "string",
              "description": "The target unit."
            }
          },
          "required": ["value", "from_unit", "to_unit"]
        }
      }
    },
    {
      "type": "function",
      "function": {
        "name": "summarize_text",
        "description": "Generates a short summary of a longer text.",
        "parameters": {
          "type": "object",
          "properties": {
            "text": {
              "type": "string",
              "description": "The full text to summarize."
            }
          },
          "required": ["text"]
        }
      }
    },
    {
      "type": "function",
      "function": {
        "name": "get_stock_price",
        "description": "Fetches the most recent stock price of a given company.",
        "parameters": {
          "type": "object",
          "properties": {
            "ticker": {
              "type": "string",
              "description": "The company’s stock ticker symbol."
            }
          },
          "required": ["ticker"]
        }
      }
    }
  ],
  "temperature": 0.7,
  "top_p": 1,
  "max_tokens": 500
}
```

<!-- ********************* -->
# OpenAI Playground Instructions to Use:
<!-- ********************* -->

1. Open [OpenAI Playground](https://platform.openai.com/playground).
2. Click the three-dot menu (⋮) in the top right corner.
3. Choose "Import configuration".
4. Paste the full JSON above.
5. Click **Import** to launch the session with tools pre-loaded.

<!-- ********************* -->
# References
<!-- ********************* -->

1. [OpenAI Playground](https://platform.openai.com/playground):
   Use this interface to test prompt structure, system message behavior, and function-calling logic.

2. [OpenAI Function Calling Documentation](https://platform.openai.com/docs/guides/function-calling):
   Explains how function calling works with GPT-4-turbo and how to define structured tool inputs.

3. [Anthropic’s MCP Reference](https://www.anthropic.com/index/mcp):
   Offers insights into the Model Context Protocol (MCP) design and structure for tool interaction.

4. [OpenAI GPT Best Practices](https://platform.openai.com/docs/guides/gpt-best-practices):
   Covers how to write effective prompts and design structured interactions.

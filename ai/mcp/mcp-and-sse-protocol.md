<!-- ********************* -->
# MCP and Server-Sent Events (SSE) in Context: HTTP, WebSockets, Kubernetes, and More
<!-- ********************* -->

**Note: This is an initial read on ChatGPT around this topic of MCP and SSE. I haven't done any edits. Often ChatGPT stuffs with wrong information. Use this just as a guide and verify every fact before assuming it is true.**

## Table of Contents

1. Introduction: Questions Addressed
2. Understanding Server-Sent Events (SSE)
3. SSE and HTTP: How They Work Together
4. Differences Between SSE and WebSockets
5. Running SSE in Kubernetes and OpenShift
6. Understanding Unidirectionality in SSE
7. Can SSE Run on Top of WebSockets?
8. MCP and SSE Integration
9. References

<!-- ********************* -->
# Introduction: Questions Addressed
<!-- ********************* -->

This article explores the use and deployment of **Server-Sent Events (SSE)** in modern environments. Specifically, it addresses the following questions:

1. How does SSE relate to HTTP and WebSockets?
2. What are the differences between SSE and WebSockets?
3. Can SSE run in environments like Kubernetes and OpenShift using HTTP routes?
4. What does unidirectional communication in SSE mean, and can it support client-to-server streaming?
5. Can SSE be run on top of WebSockets?

Each section below is dedicated to answering these questions in depth.

<!-- ********************* -->
# Understanding Server-Sent Events (SSE)
<!-- ********************* -->

SSE is a **browser-native technology** standardized as part of HTML5 that allows servers to push data to clients over HTTP. It uses the `text/event-stream` MIME type and is particularly suited for real-time updates where only server-to-client communication is needed.

Key aspects of SSE:
- Simple to implement over HTTP/1.1
- Uses long-lived HTTP connections
- Suitable for live feeds, logs, chat updates, and streaming model outputs

<!-- ********************* -->
# SSE and HTTP: How They Work Together
<!-- ********************* -->

SSE leverages standard HTTP protocols. A client sends a regular `GET` request with an `Accept: text/event-stream` header. The server responds with a `Content-Type: text/event-stream` and keeps the connection open, sending messages as needed.

There is no upgrade or handshake like in WebSockets; the whole interaction remains HTTP/1.1-compliant.

<!-- ********************* -->
# Differences Between SSE and WebSockets
<!-- ********************* -->

| Feature               | SSE                            | WebSockets                      |
|----------------------|----------------------------------|----------------------------------|
| Protocol             | HTTP/1.1 only                    | Initial HTTP handshake, then a separate WebSocket protocol |
| Direction            | Unidirectional (server → client) | Bidirectional                   |
| Complexity           | Simple to implement              | More complex setup               |
| Browser Support      | Native in most modern browsers   | Native in all modern browsers    |
| Reconnection         | Built-in auto-reconnect          | Must be implemented manually     |
| Load Balancer Support| High (uses standard HTTP)        | Can be tricky without sticky sessions |

WebSockets are better for chat apps or collaborative tools where both client and server need to exchange messages. SSE is ideal for dashboards, live updates, or streamed inference outputs.

<!-- ********************* -->
# Running SSE in Kubernetes and OpenShift
<!-- ********************* -->

Yes, SSE works in both Kubernetes and OpenShift environments.

Because SSE uses standard HTTP/1.1 connections:
- You can expose SSE endpoints via **Ingress controllers**, **Services**, or **Routes** (in OpenShift).
- Ensure that your **Ingress/Proxy/Service** supports long-lived HTTP connections and does not buffer or timeout idle connections too quickly.
- You may need to tune:
  - `read-timeout`, `idle-timeout` (in NGINX Ingress)
  - HTTP proxy buffer settings (in HAProxy, Envoy)

This makes SSE highly compatible with cloud-native environments.

<!-- ********************* -->
# Understanding Unidirectionality in SSE
<!-- ********************* -->

SSE is unidirectional: **only the server can send events to the client** once the connection is established.

Clients cannot stream data to the server through the same channel. For client-to-server communication, they must initiate a separate HTTP request (e.g., POST/PUT).

This differs from WebSockets, which allow full duplex communication over a single connection.

<!-- ********************* -->
# Can SSE Run on Top of WebSockets?
<!-- ********************* -->

No, SSE cannot run on top of WebSockets.

They are separate protocols:
- **SSE runs entirely over HTTP.**
- **WebSockets upgrade from HTTP to a new protocol.**

If you need bidirectional streaming, **WebSockets** is the preferred route. If you want server-to-client push while keeping the system HTTP-native, use **SSE**.

There are cases where developers implement **SSE-like patterns** over WebSockets manually, but these are bespoke and not standard SSE.

<!-- ********************* -->
# MCP and SSE Integration
<!-- ********************* -->

While the **Model Context Protocol (MCP)** does not prescribe any specific transport mechanism for delivering streamed data, SSE is a viable and often preferred choice for **agent tool response streaming**.

Key considerations:
- MCP tool servers may expose HTTP endpoints for executing tools.
- If the result is streamed (e.g., progressive model output), SSE can be used to return a `text/event-stream` response.
- This is particularly useful for long-running or interactive tasks where partial output improves user experience.

In such cases, the client registers a tool execution (as defined by MCP), and the backend uses SSE to stream tokens or logs while conforming to MCP’s contract.

Though not standardized as *the* MCP transport, SSE has emerged as a practical pattern in several early MCP-adjacent implementations (e.g., CrewAI’s `tool_response_type: stream`).

<!-- ********************* -->
# References
<!-- ********************* -->

1. [MDN Web Docs on Server-Sent Events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events): A comprehensive overview of the SSE API, including browser support, usage, formatting messages, and reconnection behavior.

2. [HTML Living Standard - Server-sent events](https://html.spec.whatwg.org/multipage/server-sent-events.html): The formal specification for how SSE is structured and processed by user agents, including syntax and parsing rules.

3. [Kubernetes NGINX Ingress Controller Timeouts](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/#proxy-read-timeout): Documentation on configuring NGINX ingress annotations to support long-lived HTTP streams like SSE.

4. [OpenShift Routes Documentation](https://docs.openshift.com/container-platform/latest/networking/routes/route-configuration.html): Covers how to define and manage HTTP routes in OpenShift that can carry long-lived connections such as SSE streams.

5. [WebSockets vs SSE Comparison – Ably](https://ably.com/topic/websockets-vs-sse): A comparison article that contrasts SSE and WebSockets in terms of latency, scalability, browser compatibility, and use cases.

6. [CrewAI GitHub – Tool response types](https://github.com/joaomdmoura/crewAI): Example usage of streamed tool responses in an agentic system based on MCP-like contracts, with optional streaming outputs.


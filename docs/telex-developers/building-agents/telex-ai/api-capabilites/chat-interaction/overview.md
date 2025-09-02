---
sidebar_position: 1
title: How It Works
---

# How It Works

The `/chat` endpoint enables your agents to interact with Telex AI models by sending messages and receiving AI-generated responses.

Agents can:

* Ask one-time questions (stateless)
* Hold multi-turn conversations (contextual)
* Shape model behavior via instructions
* (Soon) Receive responses as a live stream

The flexibility of this endpoint makes it ideal for building advanced **agentic systems** integrated with Telex.


## 🔗 Endpoint

**POST** `/chat`


###  Required Headers

* `X-AGENT-API-KEY`: Your API key


## How to Specify a Model

Models can be selected by specifying the model ID using one of the ways listed below:
* Using the request headers
* Using the query params
* Using the request body

### Using the request Header

#### Example

```http
X-AGENT-API-KEY: your-key
X-Model: deepseek/deepseek-r1-0528:free
```

### Using the query parameter

#### Example

```http
`POST` /chat?model=deepseek/deepseek-r1-0528:free
```

### Using the request body

#### Example

```json
{
  "model": "deepseek/deepseek-r1-0528:free",
  "messages": [
    { "role": "user", "content": "Hello!" }
  ],
  "stream": false
}
```

Agents should dynamically select models using the [Model Discovery](../model-discovery) endpoint.


## Ways to utilize This Endpoint

There are multiple ways to interact with `/chat`, depending on your use case:

1. [Stateless Chat](./chat-stateless)

   * Single message / one-turn conversation

2. [Contextual Chat](./chat-contextual)

   * Multi-turn conversations with history

3. [System & Developer Instructions](./chat-instructions)

   * Guide model behavior with roles like `system` or `developer`

4. [Streaming Chat](./chat-streaming)

   * Live, incremental responses (planned)


## Next Steps

Before sending chat requests:

* Discover available models → [Model Discovery](../model-discovery)
* Set up secure access → [Authentication](../authentication)

---

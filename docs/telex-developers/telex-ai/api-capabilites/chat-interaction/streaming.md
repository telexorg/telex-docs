---
sidebar_position: 5
id: chat-streaming
title: Streaming Chat
sidebar\_label: Streaming Chat
---

# Streaming Chat

**Streaming Chat** enables your agent to receive model responses incrementally — as they are generated — instead of waiting for the full response.

This is ideal for:

* Live agent interactions
* Interactive UIs with typing indicators
* Reducing latency for long responses


## Current Status

Streaming is **not yet supported** in Telex AI.

This page describes the future planned behavior so agents can prepare for upcoming updates.


## 🔗 Endpoint

**POST** `/telexai/chat`


## Required Headers

* `X-AGENT-API-KEY`: Your API key
* Model can be specified via:

  * `X-Model` header
  * `model` query param
  * `model` in request body


## Planned Example

```json
{
  "messages": [
    {
      "role": "user",
      "content": "Write a poem about the stars."
    }
  ],
  "stream": true
}
```

**Planned Response:** Server-Sent Events (SSE) stream of message chunks.

```text
[data stream begins]

{"role": "assistant", "content": "The stars whisper softly..."}
{"role": "assistant", "content": "... in the midnight sky."}
...
[data stream ends]
```

## Usage Notes

* Set `"stream": true` in request body to enable
* Future response will use SSE for compatibility with browsers and real-time apps
* Until officially supported, fallback to standard synchronous responses

## Next

* Return to [Chat Overview](./)
* Explore [Contextual Chat](./chat-contextual) for multi-turn conversations
* See [System & Developer Instructions](./chat-instructions) to shape agent behavior

---

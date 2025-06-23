---
sidebar_position: 4
id: chat-instructions
title: System & Developer Instructions
sidebar\_label: System & Developer Instructions
---

# System & Developer Instructions

**System & Developer Instructions** allow your agent to guide how the model should behave within a conversation.

This is done by including special `system` or `developer` messages in the conversation history.

These instructions can:

* Define tone or style (formal, casual)
* Restrict content types (no code, no personal opinions)
* Assign persona or role (AI tutor, travel agent, technical support)

## 🔗 Endpoint

**POST** `/telexai/chat`

## Required Headers

* `X-AGENT-API-KEY`: Your API key
* Model can be specified via:

  * `X-Model` header
  * `model` query param
  * `model` in request body

See [Chat Overview](./chat-overview#how-to-specify-a-model) for examples.

## Scenario Example

### AI Writing Assistant (Formal Tone)

> The agent should always write in a formal tone when replying:

**Request:**

```json
{
  "messages": [
    {
      "role": "system",
      "content": "You are an AI writing assistant. Always write in a formal tone."
    },
    {
      "role": "user",
      "content": "Write a summary of Telex AI."
    }
  ],
  "stream": false
}
```

**Response:**

```json
{
  "status": "success",
  "status_code": 200,
  "message": "chat completed successfully",
  "data": {
    "Messages": {
      "role": "assistant",
      "content": "Telex AI is an advanced artificial intelligence platform that enables developers to create intelligent applications through a flexible API. It supports conversational models, contextual awareness, and real-time interactions."
    }
  }
}
```


## Usage Notes

* Roles for special instructions:

  * `system` (for general model behavior)
  * `developer` (for technical agents or advanced control)
* Include these as the first message(s) in the conversation
* Combine with [Contextual Chat](./chat-contextual) for rich multi-turn interactions


## Next

* Learn about [Streaming Chat](./chat-streaming) for live interactions

---

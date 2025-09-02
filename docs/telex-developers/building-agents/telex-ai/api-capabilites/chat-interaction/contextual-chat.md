---
sidebar_position: 3
id: chat-contextual
title: Contextual Chat
sidebar\_label: Contextual Chat
---

# Contextual Chat

**Contextual Chat** allows your agent to maintain the conversation history — enabling multi-turn interactions where each new message builds on prior context.

This is useful for:

* Building natural, back-and-forth conversations
* Implementing agents with a short-term memory
* Applications where follow-up questions or clarifications are expected

## 🔗 Endpoint

**POST** `/chat`


## Required Headers

* `X-AGENT-API-KEY`: Your API key
* Model can be specified via:

  * `X-Model` header
  * `model` query param
  * `model` in request body

See [Chat Overview](./#how-to-specify-a-model) for examples.


## Scenario Example

### Customer Support Conversation

> An agent is helping a user troubleshoot an issue, across multiple back-and-forth messages:

**Request:**

```json
{
  "messages": [
    {
      "role": "user",
      "content": "My internet is down."
    },
    {
      "role": "assistant",
      "content": "I'm sorry to hear that! Can you tell me what lights are showing on your router?"
    },
    {
      "role": "user",
      "content": "The power and DSL lights are on, but Internet is blinking."
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
      "content": "Thanks for the details! A blinking Internet light usually indicates that your router is trying to establish a connection. Try restarting the router and let me know if the light changes."
    }
  }
}
```

## Usage Notes

* Include the full conversation history in each request
* Messages array should alternate `user` / `assistant` roles as needed
* Context length is limited by model capabilities (see [Model Discovery](../model-discovery))
* Use when agents need to appear "conversational"

## Next

* See [System & Developer Instructions](./chat-instructions) to shape agent behavior
* Learn about [Streaming Chat](./chat-streaming) for live interactions

---
